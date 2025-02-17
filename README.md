<div class="date-picker-container">
  <mat-form-field appearance="fill">
    <mat-label>Select Dates</mat-label>
    <input matInput [value]="getFormattedDates()" readonly (click)="picker.open()">
    <mat-datepicker-toggle matSuffix [for]="picker"></mat-datepicker-toggle>
    <mat-datepicker #picker (selectedChanged)="onDateSelected($event)" [dateFilter]="filterDates"></mat-datepicker>
  </mat-form-field>

  <button mat-raised-button color="primary" (click)="submit()">Submit</button>
</div>

<!-- Display selected dates -->
<div *ngIf="selectedDates.length > 0">
  <h3>Selected Dates:</h3>
  <ul>
    <li *ngFor="let date of selectedDates">{{ date | date: 'fullDate' }}</li>
  </ul>
</div>

 selectedDates: Date[] = [];

  // Disable past dates (Only allow selection from today onward)
  filterDates = (date: Date): boolean => {
    const today = new Date();
    today.setHours(0, 0, 0, 0);
    return date >= today;
  };

  // Handle Date Selection
  onDateSelected(selectedDate: Date) {
    const index = this.selectedDates.findIndex(d => d.getTime() === selectedDate.getTime());

    if (index === -1) {
      // Add date if not already selected
      this.selectedDates.push(selectedDate);
    } else {
      // Remove date if already selected
      this.selectedDates.splice(index, 1);
    }
    
    // Sort the selected dates
    this.selectedDates.sort((a, b) => a.getTime() - b.getTime());
  }

  // Format selected dates for input field
  getFormattedDates(): string {
    return this.selectedDates.map(date => date.toLocaleDateString()).join(', ');
  }

  // Submit selected dates
  submit() {
    console.log('Selected Dates:', this.selectedDates);
  }
}

.date-picker-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
  max-width: 300px;
}
