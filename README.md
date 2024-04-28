<!-- app.component.html -->

<div class="container-fluid">
  <div class="row min-vh-100 flex-column flex-md-row">
    
    <!-- Sidebar Component -->
    <app-sidebar class="col-12 col-md-3 col-xl-2 p-0 bg-dark"></app-sidebar>

    <div class="col p-0 flex-grow-1">
      <!-- Header Component -->
      <app-header class="col-12 p-0"></app-header>
      
      <!-- Grid Component (Main Content) -->
      <app-data-grid class="col p-3"></app-data-grid>
    </div>
    
  </div>
</div>
