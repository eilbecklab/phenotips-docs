## Table of contents

* [Ethnicities](#ethnicities)
* [Relatives](#relatives)
* [Work groups and study forms](#work-groups-and-study-forms)
* [User interface components](#user-interface-components)
  * [Arrays of classed objects](#arrays-of-classed-objects)
  * [ClinVar](#clinvar)
* [Miscellaneous](#miscellaneous)

## Ethnicities

The default ethnicity selector is not well adapted to US conventions. To replace
it with a simple set of radio buttons:

1. Go to /bin/edit/PhenoTips/PatientClass?editor=class
2. Click *Maternal ethnicity*.
3. Change *Pretty name* to *Maternal race*.
4. Delete everything in *Custom Display*.
5. Change *Display Type* to *radio*.
6. Uncheck *Multiple Select*.
7. Under *Values*, enter
   `Caucasian|Black|Asian|Pacific Islander|Native American|Other`
8. Repeat for *Paternal ethnicity*.
9. Click *Save and view summary*.

## Relatives

By default, PhenoTips has a wide variety of family relationships to choose from
when connecting patients. This creates a gigantic mess of duplicate and
inconsistent data. To cut down the allowed relationships to "Parent" and
"Child":

1. Go to /bin/edit/PhenoTips/RelativeClass?editor=class
2. Click *relative_type*
3. Change *Values* to `parent=Parent|child=Child`
4. Click *Save and view summary*.

## Work groups and study forms

At the end of the day, all patients in the system share the same data model.
PhenoTips "studies" are just ways to exclude certain items from the patient
entry form.

Work groups grant permission to use study forms rather than the default patient
entry form.

### Creating a study form

1. Go to *Administration* > *Studies* and enter a name in CamelCase, such as
   "NoonanSyndrome", and click *Create*. This name is permanent.
2. On the next page, enter a more user-friendly name (which you can change
   later), such as "Noonan syndrome study", and click *Save and view form*.
   Note that the Patient form configuration button does not work until after you
   have saved the form.
3. On the next page, click *Form configuration*. From here you can check boxes
   to specify which elements will appear on the form. Note that making an
   element invisible on the default patient form automatically forces those
   elements to be invisible on all other forms too.

### Creating a work group

Go to Administration > Work groups and enter a name in CamelCase, such as
EilbeckLab. Add people to the group and check the boxes next to the study forms
that they should have access to.

### Set which study form to use for a new patient

If you are in a workgroup with available studies, you will be asked every time
you create a patient record which study it belongs to. You will then be
presented with the corresponding study form instead of the default form.

### Set which study form to use for an existing patient

If you are simply moving the patient from one study to another, click on the
patient and click the edit button next to the study information in the
upper-left corner.

If the patient was created before there were any defined studies, there is no
nice way to add them to a study. You must:

1. Open the patient record and click Edit.
2. Add ?editor=object to the URL.
3. Next to *New object*, select StudyBindingClass and click Add.
4. Select the appropriate study and click *Save and view summary*.

### Make the study selector appear for all new patients

By default, the *Study* button does not appear unless there was at least one
custom study available to the user when the patient was initially created. To
make the study selector appear even if there is no option to use a custom study:

1. Go to /bin/edit/PhenoTips/PatientTemplate?editor=object
2. Next to *New object*, select StudyBindingClass and click *Add*.
3. Click *Save and view summary*.

From now on, all new patients will have a *Study* button.

## User interface components

Phenotips allows you to add new items to the patient entry form. For this to
work, you must first extend the data model, then extend the user interface. The
following example adds a field for eye color:

1.  Go to /bin/edit/PhenoTips/PatientClass?editor=class

2.  Next to *Add new property*, put `eye_color`, select an appropriate data type
    (for simplicity, you can use String), and click *Add*.

3.  Click the + sign next to the newly-created eye_color property and change
    *Pretty name* to `Eye color`.

4.  If creating a property to record a date, under *Date Format* change
    `dd/MM/yyyy HH:mm:ss` to `yyyy-MM-dd`. Then, under *Custom Display*, put:

        {{velocity}}{{html wiki=false clean=false}}
        #set ($definedFormat = "$!object.xWikiClass.get($name).getProperty('dateFormat').value")
        #if ($xcontext.action == 'edit')
        <input type="text" id="$prefix$name" title="$!definedFormat" name="$prefix$name" value="$!xwiki.formatDate($value, $definedFormat)" class="xwiki-date" alt=""/>
        #else
        <span class="date">$!xwiki.formatDate($!value, $definedFormat)</span>
        #end
        {{/html}}{{/velocity}}

    This code is necessary for the correct operation of the date picker. It was
    ripped from /bin/edit/PhenoTips/MeasurementsClass?editor=class with the alt
    attribute (which controls the page that the calendar shows first) changed to
    alt="" (i.e. always default to the page that contains the current date).

5.  Click *Save and view summary*, click the PhenoTips logo to return to the
    home page, click Administration, and click UIX.

6.  Under *New extension name*, enter `UIX_Field__eye_color` for
    the new user interface element and click *Create*.

7.  Start by filling in *Extension code*. You will need to call a Velocity
    function to display the property from the data model.

    For a **Number**, **String**, **Static List**, **TextArea**, **Email**, or
    **Boolean**:

        {{include reference="PhenoTips.PatientSheetMacros" /}}

        {{velocity}}
        #__displayFormatted('2-col', 'eye_color')
        {{/velocity}}

    For a **Date**:

        {{include reference="PhenoTips.PatientSheetMacros" /}}

        {{velocity}}
        #if ($xcontext.action == 'edit')
          $xwiki.ssx.use('XWiki.DateTimePicker', {'colorTheme' : "$xwiki.getSpacePreference('colorTheme')"})##
          $xwiki.jsx.use('XWiki.DateTimePicker')##
        #end
        #__displayFormatted('2-col', 'date_of_birth' '' 'date')
        {{/velocity}}

    Most of the time, you can copy and paste one of these examples changing only
    the name of the data model property. However, you can insert arbitrary
    content or call a different display function if you want. The display
    functions are defined in [PatientSheetMacros.xml]
    (https://github.com/phenotips/phenotips/blob/master/components/patient-data/ui/src/main/resources/PhenoTips/PatientSheetMacros.xml).

    By the way, the newline between
    `{{include reference="PhenoTips.PatientSheetMacros" /}}` and `{{velocity}}`
    matters! For whatever reason, removing this newline causes an extra
    `<p><br/></p>` to be inserted before the field, or (if the field is blank)
    to be inserted before where the field would be if it were displayed.

8.  (Optional) Under *Extension parameters*, type `title=Eye color`. This title
    (more of a label, really) will not be displayed to the user, but it will be
    displayed when you modify the patient form. It is sort of documented at
    https://phenotips.org/DevGuide/UIX#HInthepatientform

9.  Click *Save and view summary*.

10. Return to the home page and click *Administration*, then *Patient form
    structure* to modify the default form.

11. Next to *Eye color*, hold the mouse down on the cross icon to drag the user
    interface element onto the patient form, then click *Save patient form
    configuration*. Again, elements must be enabled on the default patient form
    to be able to appear on any other form.

12. (Optional) Go to *Administration* > *Studies* > *[study name]* >
    *Form configuration* to remove *Eye color* from a particular form or move it
    to a different position on the form.

## Arrays of classed objects

Some PhenoTips fields, such as "Measurements" and "List of candidate genes", are
actually arrays of objects attached to the patient. You can attach custom
objects to a patient by creating your own class and a user interface component
that displays an array of objects of that class. The following example creates a
class for eye examinations, where we are interested in the date of the exam and
the presence or absence of conjunctivitis:

1.  Go to /bin/create/Main/AllDocs and under *Page name* put `EyeExam`. Click
    *Create*, then click *Save and view summary*.

2.  Hover over the *Edit* button and click *Class*.

3.  Next to *Add new property*, put `date`, select the Date type, and click
    *Add*.

4.  Next to *Add new property*, put `conjunctivitus`, select the Boolean type,
    and click *Add*.

5.  Click the + sign next to the newly created properties. Under "date", change
    *Pretty name* to `Date`. Under "conjunctivitis", change *Pretty name* to
    `Conjunctivitis`.

6.  (Optional) To add an info button, in the *Pretty name* field add, for
    example,
    `Conjunctivitis<span class="fa fa-question-circle xHelpButton" title="Inflammation of the outermost layer of the white part of the eye, also known as pinkeye."></span>`

7.  Click *Save and view summary*, click the PhenoTips logo to return to the
    home page, click Administration, and click UIX.

8.  Under *New extension name*, enter `UIX_Field__eye_exams` for
    the new user interface element and click *Create*.

9.  Copy and paste the following into *Extension code*:

        {{include reference="PhenoTips.TabelarDataMacros" /}}

        ===Eye exams===
        {{velocity}}
        #__extradata_displayTable('Main.EyeExam', {'counter' : false, 'labels' : true, 'mode' : $xcontext.action})
        {{/velocity}}

10. (Optional) Under *Extension parameters*, type `title=Eye exams`.

11. Click *Save and view summary*.

12. Go to *Administration* > *Patient form structure*, drag and drop the "Eye
    exams" item onto the default patient form, and click *Save patient form
    configuration*.

## ClinVar

The [PhenoTipsBot](https://github.com/eilbecklab/phenotipsbot) project includes
a [script to export data from PhenoTips for ClinVar submission]
(https://github.com/eilbecklab/phenotipsbot#export-clinvarpy). To support adding
variants to patients for ClinVar submission:

1. Download [Main.ClinVarVariant.xar](Main.ClinVarVariant.xar) and
   [PhenoTips.UIX_Field__clinvar_variants.xar]
   (PhenoTips.UIX_Field__clinvar_variants.xar).
2. Go to *Administration* > *Import* and import both packages.
3. (Optional) Go to *Administration* > *Patient form structure*, drag and drop
   the "Variants for ClinVar" item from the "Genotype information" to wherever
   you'd like it, and click *Save patient form configuration*.

## Miscellaneous

* Go to /bin/Main/AllDocs to browse all XWiki documents.

* Which form sections are collapsed by default is hardcoded. To change it, go to
  /bin/edit/PhenoTips/PatientSheetCode?editor=object and under
  "JavaScriptExtension 8: Expand/collapse sections" edit or remove the following:

        $$('.chapter:not(.patient-info)').each(function(item) {
           if (!(window.location.hash && item.down(window.location.hash))) {
             item.addClassName('collapsed');
           }
        });
