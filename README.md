import { Component } from '@angular/core';

@Component({
  selector: 'app-data-grid',
  templateUrl: './data-grid.component.html',
  styleUrls: ['./data-grid.component.css'],
  imports: [
    // ... other modules here
    AgGridModule.withComponents([]),
  ]
})
export class DataGridComponent {
  columnDefs = [
    { headerName: 'Make', field: 'make' },
    { headerName: 'Model', field: 'model' },
    { headerName: 'Price', field: 'price' }
  ];

  rowData = [
    { make: 'Toyota', model: 'Celica', price: 35000 },
    { make: 'Ford', model: 'Mondeo', price: 32000 },
    { make: 'Porsche', model: 'Boxter', price: 72000 }
  ];
}


<!-- data-grid.component.html -->
<ag-grid-angular
  style="width: 100%; height: 500px;"
  class="ag-theme-alpine"
  [rowData]="rowData"
  [columnDefs]="columnDefs">
</ag-grid-angular>
