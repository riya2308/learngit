<div class="sidebar-container d-flex">
  <div class="sidebar bg-light" [ngClass]="{'sidebar-open': isOpen}">
    <ul class="nav flex-column">
      <li class="nav-item">
        <a class="nav-link" href="#">Home</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">About</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Services</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Contact</a>
      </li>
    </ul>
  </div>
  <div class="content p-4" (mouseenter)="isOpen = true" (mouseleave)="isOpen = false">
    <h1>Content Area</h1>
    <p>This is the main content area.</p>


import { Component } from '@angular/core';

@Component({
  selector: 'app-sidebar',
  templateUrl: './sidebar.component.html',
  styleUrls: ['./sidebar.component.css']
})
export class SidebarComponent {
  isOpen = false;
}


.sidebar-container {
  height: 100vh;
}

.sidebar {
  width: 200px;
  transition: width 0.3s ease-in-out;
}

.sidebar-open {
  width: 250px;
}

.content {
  flex-grow: 1;
}
