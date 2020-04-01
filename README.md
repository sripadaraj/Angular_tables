# Angular_tables
Angular 7|8|9 Edit/ Add/ Delete Rows in Material Table with using Dialogs inline Row Operation
<p> Angular Material Table component is full of features and a wide variety of options are available to create data-rich grids in a very optimized way. It provides a lot basic to advance functionalities like filter, sorting, pagination, expand-collapse, custom templating, etc.</p>
<p>
To do these operations on table rows, we will use Dialog component on Angular material. A dialog is a popup layout which is displayed to a user above all content in the center of the screen, which can easily catch user eyeballs. Dialog component can be used in many areas like getting prompt responses, showing alert messages, notifying users, getting other user inputs, etc.
o keep this simple we will have two fields in table i.e ID and Name with an extra Action column to have Edit and Delete control links.</P>

# Create a new Angular Project

``` # Install Angular CLI
$ npm install -g @angular/cli
 
# Create new project
$ npm new ngTableCRUD
 
# Enter project
$ cd ngTableCRUD
 
# Run project in VS Code
$ code 
```
# Add Angular Material in Project

<p>To use Angular material, we need to install and configure an Angular project to use its components. let’s go through some quick steps to plug in.</p>

# Install packages
<p> Run following NPM command to install @angular/material, @angular/material and @angular/material required as dependencies.</P>

```# For the latest version
$ npm install --save @angular/material @angular/cdk @angular/animations
 
# For Angular Material version 7
$ npm install --save @angular/material@7 @angular/cdk@7 @angular/animations@7
```
# Import BrowserAnimationsModule in Module
<p>Angular Material is known for beautiful animations like ripple effects 😀 so just go to app’s main module file then import BrowserAnimationsModule then add in imports array.</P>

```
//app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
 
import { AppComponent } from './app.component';
 
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
 
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    BrowserAnimationsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
# Add Material Theme

<p>
  Components used in the project are styles using CSS available in the material CSS theme file, so just add the following theme file link in styles.css available at the project root.</P>
  
  ```@import "~@angular/material/prebuilt-themes/deeppurple-amber.css";
/* @import "~@angular/material/prebuilt-themes/indigo-pink.css"; */
/* @import "~@angular/material/prebuilt-themes/pink-bluegrey.css"; */
/* @import "~@angular/material/prebuilt-themes/purple-green.css"; */
 ```
 # Import Material Modules
 <p>We are going to use Table, Dialog, FormFields, Input, Buttons as Material components in our tutorial. So we need to import these in the app.module.ts file and also add in the imports array to use them in the application.

Also, import FormsModule as we will use [(ngModel)] in our Dialog.</P>
```
//app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
 
import { AppComponent } from './app.component';
 
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
 
import { 
  MatTableModule, 
  MatDialogModule, 
  MatFormFieldModule,
  MatInputModule,
  MatButtonModule
} from '@angular/material';
 
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    BrowserAnimationsModule,
    FormsModule,
    MatTableModule,
    MatDialogModule,
    MatFormFieldModule,
    MatInputModule,
    MatButtonModule,
    MatInputModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
 ```
 # Create Material Table
 <p>
  Next, add Material Table directive in app.component.html which will take rows data object in [dataSource]

There is also a button with openDialog method called on click event.</P>
```
<!-- app.component.html -->
<div class="container text-center">
 
  <button mat-button (click)="openDialog('Add',{})" mat-flat-button color="primary">Add Row</button>
 
  <table mat-table [dataSource]="dataSource" #mytable class="my-table mat-elevation-z8">
 
    <!--- Note that these columns can be defined in any order.
        The actual rendered columns are set as a property on the row definition" -->
 
    <!-- Id Column -->
    <ng-container matColumnDef="id">
      <th mat-header-cell *matHeaderCellDef> ID. </th>
      <td mat-cell *matCellDef="let element"> {{element.id}} </td>
    </ng-container>
 
    <!-- Name Column -->
    <ng-container matColumnDef="name">
      <th mat-header-cell *matHeaderCellDef> Name </th>
      <td mat-cell *matCellDef="let element"> {{element.name}} </td>
    </ng-container>
 
    <!-- Action Column -->
    <ng-container matColumnDef="action">
      <th mat-header-cell *matHeaderCellDef> Action </th>
      <td mat-cell *matCellDef="let element" class="action-link"> 
        <a (click)="openDialog('Update',element)">Edit</a> | 
        <a (click)="openDialog('Delete',element)">Delete</a>  
      </td>
    </ng-container>
 
    <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
    <tr mat-row *matRowDef="let row; columns: displayedColumns;"></tr>
  </table>
 
</div>
```
# app.component.ts
```
<!-- app.component.html -->
<div class="container text-center">
 
  <button mat-button (click)="openDialog('Add',{})" mat-flat-button color="primary">Add Row</button>
 
  <table mat-table [dataSource]="dataSource" #mytable class="my-table mat-elevation-z8">
 
    <!--- Note that these columns can be defined in any order.
        The actual rendered columns are set as a property on the row definition" -->
 
    <!-- Id Column -->
    <ng-container matColumnDef="id">
      <th mat-header-cell *matHeaderCellDef> ID. </th>
      <td mat-cell *matCellDef="let element"> {{element.id}} </td>
    </ng-container>
 
    <!-- Name Column -->
    <ng-container matColumnDef="name">
      <th mat-header-cell *matHeaderCellDef> Name </th>
      <td mat-cell *matCellDef="let element"> {{element.name}} </td>
    </ng-container>
 
    <!-- Action Column -->
    <ng-container matColumnDef="action">
      <th mat-header-cell *matHeaderCellDef> Action </th>
      <td mat-cell *matCellDef="let element" class="action-link"> 
        <a (click)="openDialog('Update',element)">Edit</a> | 
        <a (click)="openDialog('Delete',element)">Delete</a>  
      </td>
    </ng-container>
 
    <tr mat-header-row *matHeaderRowDef="displayedColumns"></tr>
    <tr mat-row *matRowDef="let row; columns: displayedColumns;"></tr>
  </table>
 
</div>
 

#mytable is a template variable which we will use to refresh table data by calling
renderRows() method.
*matHeaderRowDef takes an array of columns we want to show in table.
matColumnDef property on the column is the same as a key in the JSON data object.
*matHeaderCellDef have the text of columns in the table header.
Now replace the following code in app.component.ts file
//app.component.ts
import { Component, ViewChild } from '@angular/core';

import { MatDialog, MatTable } from '@angular/material';
import { DialogBoxComponent } from './dialog-box/dialog-box.component';

export interface UsersData {
  name: string;
  id: number;
}

const ELEMENT_DATA: UsersData[] = [
  {id: 1560608769632, name: 'Artificial Intelligence'},
  {id: 1560608796014, name: 'Machine Learning'},
  {id: 1560608787815, name: 'Robotic Process Automation'},
  {id: 1560608805101, name: 'Blockchain'}
];
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  displayedColumns: string[] = ['id', 'name', 'action'];
  dataSource = ELEMENT_DATA;

  @ViewChild(MatTable,{static:true}) table: MatTable<any>;

  constructor(public dialog: MatDialog) {}

  openDialog(action,obj) {
    obj.action = action;
    const dialogRef = this.dialog.open(DialogBoxComponent, {
      width: '250px',
      data:obj
    });

    dialogRef.afterClosed().subscribe(result => {
      if(result.event == 'Add'){
        this.addRowData(result.data);
      }else if(result.event == 'Update'){
        this.updateRowData(result.data);
      }else if(result.event == 'Delete'){
        this.deleteRowData(result.data);
      }
    });
  }

  addRowData(row_obj){
    var d = new Date();
    this.dataSource.push({
      id:d.getTime(),
      name:row_obj.name
    });
    this.table.renderRows();
    
  }
  updateRowData(row_obj){
    this.dataSource = this.dataSource.filter((value,key)=>{
      if(value.id == row_obj.id){
        value.name = row_obj.name;
      }
      return true;
    });
  }
  deleteRowData(row_obj){
    this.dataSource = this.dataSource.filter((value,key)=>{
      return value.id != row_obj.id;
    });
  }


}
	
//app.component.ts
import { Component, ViewChild } from '@angular/core';
 
import { MatDialog, MatTable } from '@angular/material';
import { DialogBoxComponent } from './dialog-box/dialog-box.component';
 
export interface UsersData {
  name: string;
  id: number;
}
 
const ELEMENT_DATA: UsersData[] = [
  {id: 1560608769632, name: 'Artificial Intelligence'},
  {id: 1560608796014, name: 'Machine Learning'},
  {id: 1560608787815, name: 'Robotic Process Automation'},
  {id: 1560608805101, name: 'Blockchain'}
];
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  displayedColumns: string[] = ['id', 'name', 'action'];
  dataSource = ELEMENT_DATA;
 
  @ViewChild(MatTable,{static:true}) table: MatTable<any>;
 
  constructor(public dialog: MatDialog) {}
 
  openDialog(action,obj) {
    obj.action = action;
    const dialogRef = this.dialog.open(DialogBoxComponent, {
      width: '250px',
      data:obj
    });
 
    dialogRef.afterClosed().subscribe(result => {
      if(result.event == 'Add'){
        this.addRowData(result.data);
      }else if(result.event == 'Update'){
        this.updateRowData(result.data);
      }else if(result.event == 'Delete'){
        this.deleteRowData(result.data);
      }
    });
  }
 
  addRowData(row_obj){
    var d = new Date();
    this.dataSource.push({
      id:d.getTime(),
      name:row_obj.name
    });
    this.table.renderRows();
    
  }
  updateRowData(row_obj){
    this.dataSource = this.dataSource.filter((value,key)=>{
      if(value.id == row_obj.id){
        value.name = row_obj.name;
      }
      return true;
    });
  }
  deleteRowData(row_obj){
    this.dataSource = this.dataSource.filter((value,key)=>{
      return value.id != row_obj.id;
    });
  }
 
 
}
```
# App component is opening a Dialog box with DialogBoxComponent. Just run following npm command to create the component in ng CLI.

```
$ ng generate component dialog-box --skipTests=true
 ```
# Now open the app.module.ts file again then add DialogBoxComponent in
<p>
  
declarations (added by default if created in CLI) as well as entryComponents otherwise it will throw the following error as Dialog is added dynamically.</P>
``` ERROR Error: No component factory found for EditRowComponent. Did you add it to @NgModule.entryComponents?
```
<p>So our final app.module.ts file will look like this</P>
```
//app.module.ts
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
 
import { AppComponent } from './app.component';
 
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
 
import { 
  MatTableModule, 
  MatDialogModule, 
  MatFormFieldModule,
  MatInputModule,
  MatButtonModule} from '@angular/material';
 
import { DialogBoxComponent } from './dialog-box/dialog-box.component';
 
@NgModule({
  declarations: [
    AppComponent,
    DialogBoxComponent
  ],
  imports: [
    BrowserModule,
    BrowserAnimationsModule,
    FormsModule,
    MatTableModule,
    MatDialogModule,
    MatFormFieldModule,
    MatInputModule,
    MatButtonModule,
    MatInputModule
  ],
  entryComponents: [
    DialogBoxComponent
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
<P>Here we have Table data to populate and @ViewChild to get table reference with new Static option introduces in Angular 8 and is required.

The openDialog method is getting action string for Add, Update and Delete and obj as row object to pass in open method

Also subscribed to Dialog close event using afterClosed() method calling addRowData, updateRowData and deleteRowData as per event sent back from DialogBoxComponent close() method </P>

```
openDialog(action,obj) {
    obj.action = action;
    const dialogRef = this.dialog.open(DialogBoxComponent, {
      width: '250px',
      data:obj
    });
 
    dialogRef.afterClosed().subscribe(result => {
      if(result.event == 'Add'){
        this.addRowData(result.data);
      }else if(result.event == 'Update'){
        this.updateRowData(result.data);
      }else if(result.event == 'Delete'){
        this.deleteRowData(result.data);
      }
    });
  }
 ```
 # In the dialog-box.component.ts file we following code
 ```
   //dialog-box.component.ts
import { Component, Inject, Optional } from '@angular/core';
import { MatDialogRef, MAT_DIALOG_DATA } from '@angular/material';
 
export interface UsersData {
  name: string;
  id: number;
}
 
 
@Component({
  selector: 'app-dialog-box',
  templateUrl: './dialog-box.component.html',
  styleUrls: ['./dialog-box.component.css']
})
export class DialogBoxComponent {
 
  action:string;
  local_data:any;
 
  constructor(
    public dialogRef: MatDialogRef<DialogBoxComponent>,
    //@Optional() is used to prevent error if no data is passed
    @Optional() @Inject(MAT_DIALOG_DATA) public data: UsersData) {
    console.log(data);
    this.local_data = {...data};
    this.action = this.local_data.action;
  }
 
  doAction(){
    this.dialogRef.close({event:this.action,data:this.local_data});
  }
 
  closeDialog(){
    this.dialogRef.close({event:'Cancel'});
  }
 
}
```
<P>
  
MAT_DIALOG_DATA is used to get data passed in the open method’s data parameter in app.component.ts
In the dialog-box.component.html file add following code with a heading, input field and buttons to perform action based operation.</p>

```
<!-- dialog-box.component.html -->
<h1 mat-dialog-title>Row Action :: <strong>{{action}}</strong></h1>
<div mat-dialog-content>
  <mat-form-field *ngIf="action != 'Delete'; else elseTemplate">
    <input placeholder="{{action}} Name" matInput [(ngModel)]="local_data.name">
  </mat-form-field>
  <ng-template #elseTemplate>
    Sure to delete <b>{{local_data.name}}</b>?
  </ng-template>
</div>
<div mat-dialog-actions>
  <button mat-button (click)="doAction()">{{action}}</button>
  <button mat-button (click)="closeDialog()" mat-flat-button color="warn">Cancel</button>
</div>
```
<p>
 That’s it now run the application using</P>
 
  ```1
2
	
  $ ng serve --open
  ```
  
 
