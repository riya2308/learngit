<div class="row-layout row">
  <igx-grid #igxGrid1 [data]="yourDataArray" [allowFiltering]="true" filterMode="excelStyleFilter"
            [autoGenerate]="false" width="98%" height="96%" displayDensity="cosy"
            primaryKey="id" class="grid">
            
    

  </igx-grid>
</div>

<igx-column field="id" header="ID" title="ID" dataType="string" [sortable]="true" [groupable]="true"></igx-column>
    <igx-column field="eoinid" header="EOIN ID" title="EOIN ID" dataType="string" [sortable]="true" [groupable]="true"></igx-column>
    <igx-column field="percentage" header="Percentage" title="Percentage" dataType="number" [sortable]="true" [groupable]="true"></igx-column>
