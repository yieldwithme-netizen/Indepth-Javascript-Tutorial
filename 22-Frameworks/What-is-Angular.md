# What is Angular?

Angular is a powerful, opinionated JavaScript framework for building single-page applications (SPAs) and progressive web apps (PWAs). Developed and maintained by Google, it provides a complete solution for frontend development.

## Definition

Angular is a platform and framework for building client-side applications using HTML, CSS, and TypeScript. It features a component-based architecture, dependency injection, reactive programming with RxJS, and a comprehensive CLI for development workflows.

## Core Concepts

### 1. Components
```typescript
// counter.component.ts
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
    selector: 'app-counter',
    template: `
        <div class="counter">
            <h2>{{ title }}</h2>
            <p>Count: {{ count }}</p>
            <button (click)="increment()">+</button>
            <button (click)="decrement()">-</button>
        </div>
    `,
    styles: [`
        .counter { padding: 20px; }
        button { margin: 5px; }
    `]
})
export class CounterComponent {
    @Input() title: string = 'Counter';
    @Input() initialCount: number = 0;
    @Output() countChange = new EventEmitter<number>();
    
    count: number = 0;
    
    ngOnInit() {
        this.count = this.initialCount;
    }
    
    increment() {
        this.count++;
        this.countChange.emit(this.count);
    }
    
    decrement() {
        this.count--;
        this.countChange.emit(this.count);
    }
}
```

### 2. Services and Dependency Injection
```typescript
// user.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

export interface User {
    id: number;
    name: string;
    email: string;
}

@Injectable({
    providedIn: 'root'
})
export class UserService {
    private apiUrl = '/api/users';
    
    constructor(private http: HttpClient) {}
    
    getUsers(): Observable<User[]> {
        return this.http.get<User[]>(this.apiUrl);
    }
    
    getUser(id: number): Observable<User> {
        return this.http.get<User>(`${this.apiUrl}/${id}`);
    }
    
    createUser(user: User): Observable<User> {
        return this.http.post<User>(this.apiUrl, user);
    }
}
```

### 3. Routing
```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { UserListComponent } from './users/user-list.component';
import { UserDetailComponent } from './users/user-detail.component';
import { AuthGuard } from './guards/auth.guard';

const routes: Routes = [
    { path: '', component: HomeComponent },
    { path: 'users', component: UserListComponent, canActivate: [AuthGuard] },
    { path: 'users/:id', component: UserDetailComponent },
    { path: '**', redirectTo: '' }
];

@NgModule({
    imports: [RouterModule.forRoot(routes)],
    exports: [RouterModule]
})
export class AppRoutingModule {}
```

### 4. Reactive Forms
```typescript
// registration.component.ts
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
    selector: 'app-registration',
    template: `
        <form [formGroup]="registrationForm" (ngSubmit)="onSubmit()">
            <div>
                <label>Name:</label>
                <input formControlName="name">
                <div *ngIf="registrationForm.get('name')?.invalid && registrationForm.get('name')?.touched">
                    Name is required
                </div>
            </div>
            
            <div>
                <label>Email:</label>
                <input formControlName="email" type="email">
            </div>
            
            <div>
                <label>Password:</label>
                <input formControlName="password" type="password">
            </div>
            
            <button type="submit" [disabled]="registrationForm.invalid">
                Register
            </button>
        </form>
    `
})
export class RegistrationComponent {
    registrationForm: FormGroup;
    
    constructor(private fb: FormBuilder) {
        this.registrationForm = this.fb.group({
            name: ['', [Validators.required, Validators.minLength(2)]],
            email: ['', [Validators.required, Validators.email]],
            password: ['', [Validators.required, Validators.minLength(8)]]
        });
    }
    
    onSubmit() {
        if (this.registrationForm.valid) {
            console.log(this.registrationForm.value);
        }
    }
}
```

### 5. HTTP Client
```typescript
// api.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError, retry } from 'rxjs/operators';

@Injectable({
    providedIn: 'root'
})
export class ApiService {
    private baseUrl = 'https://api.example.com';
    
    constructor(private http: HttpClient) {}
    
    getData<T>(endpoint: string): Observable<T> {
        return this.http.get<T>(`${this.baseUrl}/${endpoint}`)
            .pipe(
                retry(2),
                catchError(this.handleError)
            );
    }
    
    private handleError(error: HttpErrorResponse) {
        let errorMessage = 'Unknown error occurred';
        if (error.error instanceof ErrorEvent) {
            errorMessage = `Client Error: ${error.error.message}`;
        } else {
            errorMessage = `Server Error: ${error.status} - ${error.message}`;
        }
        console.error(errorMessage);
        return throwError(() => new Error(errorMessage));
    }
}
```

## Common Use Cases

### 1. Building a Dashboard
```typescript
// dashboard.component.ts
@Component({
    selector: 'app-dashboard',
    template: `
        <div class="dashboard">
            <app-sidebar></app-sidebar>
            <main class="content">
                <app-header></app-header>
                <div class="widgets">
                    <app-stat-widget 
                        *ngFor="let stat of stats"
                        [title]="stat.title"
                        [value]="stat.value"
                    ></app-stat-widget>
                </div>
            </main>
        </div>
    `
})
export class DashboardComponent implements OnInit {
    stats: Stat[] = [];
    
    constructor(private dashboardService: DashboardService) {}
    
    ngOnInit() {
        this.dashboardService.getStats().subscribe(stats => {
            this.stats = stats;
        });
    }
}
```

### 2. Lazy Loading Modules
```typescript
// app-routing.module.ts
const routes: Routes = [
    {
        path: 'admin',
        loadChildren: () => import('./admin/admin.module')
            .then(m => m.AdminModule),
        canActivate: [AuthGuard]
    }
];
```

## Common Mistakes

1. **Not using trackBy with ngFor** - Poor performance:
   ```html
   <!-- Wrong -->
   <div *ngFor="let item of items">{{ item.name }}</div>
   
   <!-- Correct -->
   <div *ngFor="let item of items; trackBy: trackById">
       {{ item.name }}
   </div>
   ```
   ```typescript
   trackById(index: number, item: Item): number {
       return item.id;
   }
   ```

2. **Subscribing without unsubscribing** - Memory leaks:
   ```typescript
   // Wrong
   ngOnInit() {
       this.userService.getUsers().subscribe(users => {
           this.users = users;
       });
   }
   
   // Correct
   ngOnInit() {
       this.subscription = this.userService.getUsers().subscribe(users => {
           this.users = users;
       });
   }
   
   ngOnDestroy() {
       this.subscription.unsubscribe();
   }
   ```

3. **Direct DOM manipulation** - Violates Angular patterns:
   ```typescript
   // Wrong
   document.getElementById('myElement').style.color = 'red';
   
   // Correct
   @ViewChild('myElement') myElement: ElementRef;
   
   changeColor() {
       this.myElement.nativeElement.style.color = 'red';
   }
   ```

## Related Topics

- [[What-is-Vue]]
- [[Compile-TypeScript]]
- [[What-is-TsConfig]]

## Quick Revision

- Angular is a full-featured framework by Google
- Component-based architecture with TypeScript
- Built-in dependency injection, routing, and forms
- RxJS for reactive programming and async operations
- Angular CLI for project setup and development
- Uses decorators: `@Component`, `@Injectable`, `@Input`, `@Output`
- Supports lazy loading and code splitting
- Great for large enterprise applications
- Steeper learning curve but comprehensive solution
