# 🛡️ Angular Route Guards – A Practical Guide

## 📘 What are Route Guards?

**Route Guards** in Angular are interfaces that allow you to control whether a user can:

- Enter a route (`canActivate`)
- Leave a route (`canDeactivate`)
- Load a module (`canLoad`)
- Access child routes (`canActivateChild`)
- Run a route resolver (`resolve`)

They act like **middleware** between the router and the component.

---

## ✅ Why Use Route Guards?

Use route guards to:

- Protect routes from unauthorized users
- Prevent loss of unsaved form data
- Pre-fetch data before navigating
- Control access to lazy-loaded modules

---

## 📦 Types of Route Guards

| Guard Type         | Purpose                            |
| ------------------ | ---------------------------------- |
| `canActivate`      | Prevent entering a route           |
| `canDeactivate`    | Prevent leaving a route            |
| `canActivateChild` | Protect child routes               |
| `canLoad`          | Prevent lazy-loading of modules    |
| `resolve`          | Fetch data before route activation |

---

## 🔐 1. `canActivate` Guard – Protect Routes

### ✅ Use Case

Block unauthenticated users from accessing `/dashboard`.

### Implementation

**auth.guard.ts**

```ts
import { Injectable } from "@angular/core";
import { CanActivate, Router } from "@angular/router";

@Injectable({ providedIn: "root" })
export class AuthGuard implements CanActivate {
  constructor(private router: Router) {}

  canActivate(): boolean {
    const isLoggedIn = !!localStorage.getItem("token");
    if (!isLoggedIn) {
      this.router.navigate(["/login"]);
      return false;
    }
    return true;
  }
}
```

**app-routing.module.ts**

```ts
{ path: 'dashboard', component: DashboardComponent, canActivate: [AuthGuard] }
```

---

## 🔄 2. `canDeactivate` Guard – Prevent Leaving with Unsaved Changes

### ✅ Use Case

Warn users before leaving a form with unsaved changes.

### Step 1: Create an Interface

```ts
export interface CanComponentDeactivate {
  canDeactivate: () => boolean | Observable<boolean>;
}
```

### Step 2: Implement in Component

```ts
export class EditProfileComponent implements CanComponentDeactivate {
  formChanged = true;

  canDeactivate() {
    return !this.formChanged || confirm("Discard changes and leave?");
  }
}
```

### Step 3: Create the Guard

```ts
import { CanDeactivate } from "@angular/router";
import { Injectable } from "@angular/core";
import { CanComponentDeactivate } from "./can-component-deactivate";

@Injectable({ providedIn: "root" })
export class UnsavedChangesGuard
  implements CanDeactivate<CanComponentDeactivate>
{
  canDeactivate(component: CanComponentDeactivate): boolean {
    return component.canDeactivate();
  }
}
```

### Step 4: Apply the Guard

```ts
{ path: 'edit-profile', component: EditProfileComponent, canDeactivate: [UnsavedChangesGuard] }
```

---

## 👶 3. `canActivateChild` – Protect Child Routes

### ✅ Use Case

Ensure child routes are secure if parent route is protected.

```ts
{
  path: 'admin',
  component: AdminComponent,
  canActivateChild: [AuthGuard],
  children: [
    { path: 'users', component: UsersComponent },
    { path: 'settings', component: SettingsComponent }
  ]
}
```

---

## 🚫 4. `canLoad` – Prevent Lazy-Loaded Modules from Loading

### ✅ Use Case

Don’t even load admin module if the user isn’t an admin.

```ts
@Injectable({ providedIn: "root" })
export class AdminGuard implements CanLoad {
  canLoad(): boolean {
    const isAdmin = localStorage.getItem("role") === "admin";
    return isAdmin;
  }
}
```

```ts
{ path: 'admin', loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule), canLoad: [AdminGuard] }
```

---

## 📥 5. `resolve` – Pre-Fetch Data Before Navigation

### ✅ Use Case

Load user data before showing the profile page.

```ts
@Injectable({ providedIn: "root" })
export class UserResolver implements Resolve<User> {
  constructor(private userService: UserService) {}

  resolve(route: ActivatedRouteSnapshot): Observable<User> {
    const id = route.paramMap.get("id");
    return this.userService.getUserById(id!);
  }
}
```

```ts
{ path: 'profile/:id', component: ProfileComponent, resolve: { userData: UserResolver } }
```

In component:

```ts
ngOnInit() {
  this.route.data.subscribe(data => {
    this.user = data['userData'];
  });
}
```

---

## 🧠 Summary

| Guard              | Purpose                               | Example Use Case                     |
| ------------------ | ------------------------------------- | ------------------------------------ |
| `canActivate`      | Block entering route                  | Check if user is logged in           |
| `canDeactivate`    | Block leaving route                   | Prevent loss of unsaved form data    |
| `canActivateChild` | Secure nested child routes            | Admin section                        |
| `canLoad`          | Prevent module from being lazy-loaded | Restrict loading of admin module     |
| `resolve`          | Pre-fetch data before navigation      | Load user profile before route loads |

---

## 📚 Bonus Tips

- All guards can return `boolean`, `Observable<boolean>`, or `Promise<boolean>`.
- Use `combineLatest` or `switchMap` in guards for async checks.
- Create a base form class to reuse `canDeactivate()` logic across multiple components.

---

Happy Routing! 🚀
