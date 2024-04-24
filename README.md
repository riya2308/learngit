// src/app/app.component.ts
import { Component, ViewChild, OnInit } from '@angular/core';
import { IgxDockManagerComponent, IDockManagerLayout } from 'igniteui-angular';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  @ViewChild('dockManager', { static: true })
  public dockManager: IgxDockManagerComponent;

  public layout: IDockManagerLayout = {
    rootPane: {
      type: 'splitPane',
      orientation: 'horizontal',
      panes: [
        {
          type: 'dockPanel',
          allowedDock: 'left',
          isPinned: false,
          size: 200,
          contentId: 'sideContent'
        },
        {
          type: 'contentPane',
          contentId: 'mainContent'
        }
      ]
    }
  };

  ngOnInit() {
    this.dockManager.layout = this.layout;
  }
}
