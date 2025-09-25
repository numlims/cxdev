package changes and code snippets when going from cx 3 to cx 4.

## package changes

package name changes:
`de.kairos.centraxx.workflow.centraxxdata.dataobjects` becomes
`de.kairos.centraxx.workflow.data.dataobjects`,
`de.kairos.centraxx.workflow.data.services.*` becomes
`de.kairos.centraxx.workflow.services.*`.

some ui classes move from `com.vaadin.ui` to `de.kairos.ui.components`
and get 'Kairos' prefixed to their name: `TextField` as
`KairosTextField`, `Label` as `KairosLabel`, `ComboBox` as
`KairosComboBox`, `GridLayout` as `KairosGridLayout`.

ui classes that are not moving to `de.kairos.ui`:
`com.vaadin.ui.Button` becomes
`com.vaadin.flow.component.button.Button`, `Checkbox` to
`com.vaadin.flow.component.checkbox.Checkbox`, `Component` to
`com.vaadin.flow.component.Component`, `com.vaadin.ui.HorizontalLayout
` to `com.vaadin.flow.component.orderedlayout.HorizontalLayout`,
`VerticalLayout` to
`com.vaadin.flow.component.orderedlayout.VerticalLayout`.

`de.kairos.centraxx.workflow.component.AmountField` to
`de.kairos.centraxx.workflow.component.KairosAmountField`.

`import de.kairos.centraxx.workflow.components.Popup;` to
`import de.kairos.ui.components.Popup;`

`import de.kairos.centraxx.workflow.centraxxdata.dataobjects.catalog.CatalogDataObject;` to
`import de.kairos.centraxx.workflow.data.dataobjects.catalog.CatalogDataObject;`

The most important change:

`import de.kairos.centraxx.workflow.components.UIComponentFactory;` to
`import de.kairos.centraxx.workflow.component.UIComponentFactory;`
`UIComponentFactory` has NO more `createLabel`, `createCheckBox` etc. 

Check if true: 
`import de.kairos.centraxx.workflow.components.WorkflowUtils;`
is now `import de.kairos.centraxx.workflow.util.Utils;`

there doesn't seem to be a com.vaadin.ui.Panel anymore.

`import com.vaadin.data.Property.ValueChangeEvent;` (Vaadin 7) to
`import com.vaadin.flow.component.AbstractField.ComponentValueChangeEvent;` (Vaadin 14)
`import com.vaadin.data.Property.ValueChangeListener;` (Vaadin 7) to
`import com.vaadin.flow.component.HasValue.ValueChangeListener;` (Vaadin 14)
Notification moves to `de.kairos.ui.components.Notification`.

Unit moves to `com.vaadin.flow.component.Unit`.

kairos `DateTimeField` becomes
`de.kairos.centraxx.workflow.component.KairosPrecisionDateField`.

`DataObjectUtils` and `ExportFileUtils` to `de.kairos.centraxx.workflow.util`.

`de.kairos.centraxx.workflow.components` drops an s, to
`de.kairos.centraxx.workflow.component`.

## field validation

`FieldValidationList` is no more, there is a
`de.kairos.centraxx.workflow.validator.MandatoryFieldValidator`.

use like this:

```
  private final MandatoryFieldValidator validator = new
  MandatoryFieldValidator();
  validator.addValidatable(pipettingSchemaCombo, rackTypeCombo,
  rackIdField, derivalDateField, repositionDateField);
  ...
  return validator.isValid();  
```

## workflow utils

WorkflowUtils seems to be no more
(`WorkflowUtils.setValuesToComboBoxByDataObject`), maybe
`de.kairos.centraxx.workflow.component.simpleassigncomponent.KairosComboBoxItem`.

## message resources

in cx3 we use a local utils.MessageResources. maybe continue using
this, otherwise kairos `MessageResources` are in
`de.kairos.centraxx.workflow.util.MessageResources`.

use them like:

```
  private MessageResources messages = new
  MessageResources(this.getClass());
  messages.get("scan.csv.upload")  
```

## table moving to grid?

wo ist `com.vaadin.ui.Table` jetzt? `import
de.kairos.ui.components.KairosTable`?

but could the KairosTable hold custom rows or just display strings
from dataobject like in SamplePipettingUI.java?

```
scannerTable.addColumn(ScannerDataObject::getManufacturer).setHeader("bla
header")
```

in cx3 we used vaadin Table as base for SampleTable.

vaadin Table apparently is no more, use Grid instead:
https://vaadin.com/blog/mission-rip-table-migrate-to-grid-intro

and https://vaadin.com/blog/mission-rip-table-migrate-to-grid-basic

Grid doc:
https://vaadin.com/api/platform/14.8.20/com/vaadin/flow/component/grid/Grid.html

some Grid code examples:
https://vaadin.com/docs/latest/components/grid

single rows can be edited upon click:
https://vaadin.com/docs/latest/components/grid/inline-editing

what about editing all rows? maybe with ComponentRenderer:
https://vaadin.com/docs/latest/components/grid/renderers

like this:


```
grid.addColumn(new ComponentRenderer<>(person -> {

    // text field for entering a new name for the person
    TextField name = new TextField("Name");
    name.setValue(person.getName());

    // button for saving the name to backend
    Button update = new Button("Update", event -> {
        person.setName(name.getValue());
        grid.getDataProvider().refreshItem(person);
    });

    // button that removes the item
    Button remove = new Button("Remove", event -> {
        ListDataProvider<Person> dataProvider =
                 (ListDataProvider<Person>) grid
                 .getDataProvider();
        dataProvider.getItems().remove(person);
        dataProvider.refreshAll();
    });

    // layouts for placing the text field on top
    // of the buttons
    HorizontalLayout buttons =
       new HorizontalLayout(update, remove);
    return new VerticalLayout(name, buttons);
})).setHeader("Actions");
```

## click listeners

`com.vaadin.ui.Button.ClickListener` seems to be no more,
`this.someButton.addClickListener(l -> onScanButtonClicked())` seems
to work out of the box?

## combo box

ComboBoxes are now created like this:

```
KairosComboBox<CatalogDataObject> myBox = new KairosComboBox<>(CatalogDataObject::getName, true)
```

and items are set like this:

```
myBox.setItems(myDataObjects);
```

## text field

to create a TextField with label, say:

```
myTextField = new KairosTextField();
myTextField.setLabel("my label");
```

## documentation for `de.kairos.ui`

kairos doesn't offer a javadoc for classes like `KairosComboBox` in
`de.kairos.ui` (part of vaadin flow components jar) and says it
doesn't plan to do so in the future.

## specify lowagie

specify lowagie in the pom.xml for com.lowagie.text.* to load:

    <dependency>
      <groupId>com.lowagie</groupId>
      <artifactId>itext</artifactId>
      <version>2.1.7</version>
    </dependency>

## file export

class `de.kairos.centraxx.workflow.helper.ExportFileUtils` has moved to
`de.kairos.centraxx.workflow.util.ExportFileUtils`.

ExportFileUtils doesn't have exportFile() anymore, maybe it moved to
FileDownloader.download()?

## getting and setting workflow task variables

`getWorkflowVariable()` seems to be callable from ui and controller.

how does setting work? with `addWorkflowVariable()` or
`setVariablePool()`? they only seem to be callable from the
controller, but maybe that's ok.
