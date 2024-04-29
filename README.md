<div class="row">
  <div class="col-md-11 text-right">
    <button type="button" class="btn btn-primary mt-2 mb-3">Approve</button>
  </div>

</div>

<div class="form-check">
  <input class="form-check-input" type="checkbox" id="selectAllCheckbox" (change)="selectAll($event)">
  <label class="form-check-label" for="selectAllCheckbox">
    Select All
  </label>
</div>

selectAll(event) {
  const isChecked = event.target.checked;
  // Assuming `rows` is the variable that contains the grid data
  this.rows.forEach(row => {
    row.selected = isChecked;
  });
}
