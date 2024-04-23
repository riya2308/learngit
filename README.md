import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-grid-component',
  templateUrl: './grid-component.component.html',
  styleUrls: ['./grid-component.component.scss']
})
export class GridComponent implements OnInit {

  public data: Array<{ id: number, eoinid: number, percentage: number }>;

  constructor() { }

  ngOnInit(): void {
    this.data = [
      { id: 1, eoinid: 101, percentage: 75 },
      { id: 2, eoinid: 102, percentage: 88 },
      { id: 3, eoinid: 103, percentage: 92 },
      // ... more data
    ];
  }

}
