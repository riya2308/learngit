<igx-dock-manager #dockManager>
  <igx-pane [contentId]="'content1'" [header]="'Content 1'">
    <!-- Your content for pane 1 -->
  </igx-pane>
  <igx-pane [contentId]="'content2'" [header]="'Content 2'" [isPinned]="true">
    <!-- Your content for pane 2 -->
  </igx-pane>
</igx-dock-manager>


import { Component, ViewChild } from '@angular/core';
import { IgxDockManagerComponent, IgxDockManagerPaneType, IgxSplitPaneOrientation } from '@infragistics/igniteui-dockmanager';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  @ViewChild('dockManager', { static: true }) public dockManager: IgxDockManagerComponent;

  ngAfterViewInit() {
    this.dockManager.layout = {
      rootPane: {
        type: IgxDockManagerPaneType.splitPane,
        orientation: IgxSplitPaneOrientation.horizontal,
        panes: [
          {
            type: IgxDockManagerPaneType.splitPane,
            orientation: IgxSplitPaneOrientation.vertical,
            panes: [
              {
                type: IgxDockManagerPaneType.contentPane,
                contentId: 'content1'
              },
              {
                type: IgxDockManagerPaneType.contentPane,
                contentId: 'content2',
                isPinned: true
              }
            ]
          }
        ]
      }
    };
  }
}
