## Table of contents

* [Registration](#registration)
* [Ethnicities](#ethnicities)
* [Relatives](#relatives)
* [Work groups and study forms](#work-groups-and-study-forms)
* [Phenotype list](#phenotype-list)
* [User interface components](#user-interface-components)
  * [Typical components](#typical-components)
  * [Dates](#dates)
  * [Ternaries](#ternaries)
  * [Lists of options](#lists-of-options)
  * [Instructional text](#instructional-text)
  * [Arrays of classed objects](#arrays-of-classed-objects)
  * [Files and images](#files-and-images)
  * [ClinVar](#clinvar)
* [Miscellaneous](#miscellaneous)

## Registration

By default, anyone in the world can register for an account on your PhenoTips
site, although they can only view patient data that they input themselves. To
disallow new account creation except by site administrators:

1.  Go to *Administration* > *Rights*.
2.  In the *XWikiAllGroup* line, double-click the *Register* checkbox. After
    that it should display a red no symbol (🚫).
3.  Click the *Users* radio button and repeat step 2 for the
    *Unregistered Users* line.

The new setting takes effect as soon as you see the no symbol.

## Ethnicities

The default ethnicity selector is not well adapted to US conventions. It has too
many options, and in the USA the term *ethnicity* is usually reserved for
describing whether a person is Hispanic or non-Hispanic.

To replace the ethnicity selector with a simple
[set of checkboxes](#lists-of-options):

1.  Go to /bin/edit/PhenoTips/PatientClass?editor=class

2.  Click *Maternal ethnicity*.

3.  (Optional) Change *Pretty name* to `Maternal race`. Because this is a
    builtin PhenoTips field, changing the pretty name does not actually change
    the user interface (see the instructions below).

4.  Delete everything in *Custom Display*.

5.  Change *Display Type* to *checkbox*.

6.  Under *Values*, enter
    `Caucasian|Black|Asian|Pacific Islander|Native American|Other`

7.  Repeat for *Paternal ethnicity*.

8.  Click *Save and view summary*.

To change the word *ethnicity* to *race* on the user interface:

1.  Go to /bin/edit/PhenoTips/TranslationOverrides?editor=object

2.  Next to *New object*, select TranslationDocumentClass and click *Add*.

3.  Under *Scope*, select WIKI.

4.  Click *Save and view summary*.

5.  Click *Edit* (to get the wiki text editor).

6.  Copy and paste the following into *Content*:

        PhenoTips.PatientClass_maternal_ethnicity=Maternal race
        PhenoTips.PatientClass_paternal_ethnicity=Paternal race

7. Click *Save and view summary*.

For more information, read
[Overriding translations](https://phenotips.org/FAQ/Overriding+translations) in
the [PhenoTips FAQ](https://phenotips.org/FAQ/).

Because these instructions do not change the `maternal_ethnicity` and
`paternal_ethnicity` property names, they preserve compatibility with the
standard PhenoTips data model.

If you also want to capture whether the patient is Hispanic or Latino,
add a new [ternary](#ternaries) named `hispanic_or_latino`.

## Relatives

By default, PhenoTips has a wide variety of family relationships to choose from
when connecting patients. This creates a gigantic mess of duplicate and
inconsistent data. To cut down the allowed relationships to "Parent" and
"Child":

1.  Go to /bin/edit/PhenoTips/RelativeClass?editor=class
2.  Click *relative_type*
3.  Change *Values* to `parent=Parent|child=Child`
4.  Click *Save and view summary*.

## Work groups and study forms

At the end of the day, all patients in the system share the same data model.
PhenoTips "studies" are just ways to exclude certain items from the patient
entry form.

Work groups grant users permission to use study forms rather than the default
patient entry form. They are not to be confused with "user groups", which grant
other permissions.

### Creating a study form

1.  Go to *Administration* > *Studies* and enter a name in CamelCase, such as
    "NoonanSyndrome", and click *Create*. This name is permanent.
2.  On the next page, enter a more user-friendly name (which you can change
    later), such as "Noonan syndrome study", and click *Save and view form*.
    Note that the Patient form configuration button does not work until after
    you have saved the form.
3.  On the next page, click *Form configuration*. From here you can check boxes
    to specify which elements will appear on the form. Note that making an
    element invisible on the default patient form automatically forces those
    elements to be invisible on all other forms too.

### Creating a work group

Go to *Administration* > *Work groups* and enter a name in CamelCase, such as
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

1.  Open the patient record and click *Edit*.
2.  Add ?editor=object to the URL.
3.  Next to *New object*, select StudyBindingClass and click *Add*.
4.  Select the appropriate study and click *Save and view summary*.

### Make the study selector appear for all new patients

By default, the *Study* button does not appear unless there was at least one
custom study available to the user when the patient was initially created. To
make the study selector appear even if there is no option to use a custom study:

1.  Go to /bin/edit/PhenoTips/PatientTemplate?editor=object
2.  Next to *New object*, select StudyBindingClass and click *Add*.
3.  Click *Save and view summary*.

From now on, all new patients will have a *Study* button.

## Phenotype list

When selecting phenotypes, the user has the option of either searching or
browsing a categorized list. It's possible to change the phenotypes in the
categorized list. For example, to only display eye defects and make the user
use the "Quick phenotype search" box for everything else:

1.  Go to /bin/create/PhenoTips/ and under *Page name* put
    `Eye defect phenotype configuration`. Click *Create*, then click
    *Save and view summary*.

2.  Hover over the *Edit* button and click *Objects*.

3.  Next to *New object*, select PhenoTips.PhenotypeMappingClass, and click
    *Add*.

4.  Hover over the *Edit* button and click *Wiki*.

5.  Copy and paste the following into *Content*:

        {
          "phenotype" : [
            {
              "type"       : "section",
              "title"      : "Eye Defects",
              "categories" : ["HP:0000478"],
              "data"       : [
                {"id" : "HP:0000505"},
                {"id" : "HP:0000481"},
                {"id" : "HP:0000589"},
                {"id" : "HP:0000593"},
                {"id" : "HP:0000518"},
                {"id" : "HP:0000479"},
                {"id" : "HP:0000587"},
                {"id" : "HP:0000568"},
                {"id" : "HP:0000639"},
                {"id" : "HP:0000486"},
                {"id" : "HP:0000601"},
                {"id" : "HP:0000316"},
              ]
            },
          ]
        }

    Then click *Save and view summary*.

6.  Click the PhenoTips logo to return to the home page, click *Administration*,
    click *Studies*, click the study form you want to modify, and click
    *Form configuration*.

7.  On the right side of the page, select *Eye defect configuration*, then click
    *Save and view summary*.

The mapping code is JSON, and this example is just
/bin/edit/PhenoTips/Generic+phenotype+configuration with everything except the
"Eye Defects" section removed. Each section that you define here has a
"categories" property that controls the search box for that section. Only
terms specified in this property and their children will appear in the search
results for the section search box, although the "Quick phenotype search" will
still search the entire ontology.

## User interface components

### Typical components

Phenotips allows you to add new items to the patient entry form. For this to
work, you must first extend the data model, then extend the user interface. The
following example adds a field for eye color:

1.  Go to /bin/edit/PhenoTips/PatientClass?editor=class

2.  Next to *Add new property*, put `eye_color`, select an appropriate data type
    (for simplicity, you can use *String*), and click *Add*.

3.  Click the + sign next to the newly-created eye_color property and change
    *Pretty name* to `Eye color`.

4.  Click *Save and view summary*, click the PhenoTips logo to return to the
    home page, click *Administration*, and click *UIX*.

5.  Under *New extension name*, enter `UIX_Field__eye_color` for
    the new user interface element and click *Create*.

6.  Start by filling in *Extension code*. You will need to call a Velocity
    function to display the property from the data model. This is the code that
    you would typically use:

        {{include reference="PhenoTips.PatientSheetMacros" /}}

        {{velocity}}
        #__displayFormatted('2-col', 'eye_color')
        {{/velocity}}

    Most of the time, you can copy and paste this examples changing only the
    name of the data model property. However, you can insert arbitrary content
    or call a different display function if you want. The display functions are
    defined in [PatientSheetMacros.xml]
    (https://github.com/phenotips/phenotips/blob/master/components/patient-data/ui/src/main/resources/PhenoTips/PatientSheetMacros.xml).

    For example, to display the field below its label instead of to the side,
    use:

        #__displayFormatted('', 'eye_color', '', '', 'section')

    By the way, the newline between
    `{{include reference="PhenoTips.PatientSheetMacros" /}}` and `{{velocity}}`
    matters! For whatever reason, removing this newline causes an extra
    `<p><br/></p>` to be inserted before the field, or (if the field is blank)
    to be inserted before where the field would be if it were displayed.

7.  (Optional) Under *Extension parameters*, type `title=Eye color`. This title
    (more of a label, really) will not be displayed to the user, but it will be
    displayed when you modify the patient form. It is sort of documented at
    https://phenotips.org/DevGuide/UIX#HInthepatientform

8.  Click *Save and view summary*.

9.  Return to the home page and click *Administration*, then *Patient form
    structure* to modify the default form.

10. Next to *Eye color*, hold the mouse down on the cross icon to drag the user
    interface element onto the patient form, then click *Save patient form
    configuration*. Again, elements must be enabled on the default patient form
    to be able to appear on any other form.

11. (Optional) Go to *Administration* > *Studies* > *[study name]* >
    *Form configuration* to remove *Eye color* from a particular form or move it
    to a different position on the form.

### Dates

If creating a property to record a date, there are a couple of differences. For
example, to create a property to record the patient's conception date:

1.  When creating the property, set its type to *Date*.

2.  After creating the property, under *Custom Display*, put:

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

3.  (Optional) If you don't want the date to be filled in automatically, uncheck
    *Empty is today*.

4.  When creating the user interface extension, under *Extension code*, put:

        {{include reference="PhenoTips.PatientSheetMacros" /}}

        {{velocity}}
        #if ($xcontext.action == 'edit')
          $xwiki.ssx.use('XWiki.DateTimePicker', {'colorTheme' : "$xwiki.getSpacePreference('colorTheme')"})##
          $xwiki.jsx.use('XWiki.DateTimePicker')##
        #end
        #__displayFormatted('2-col', 'conception_date' '' 'date')
        {{/velocity}}

### Ternaries

A ternary is a question with one of three answers: *Yes*, *No*, and
*Not Applicable*. The third option makes ternaries more flexible than checkboxes
and they integrate better with PhenoTips's look and feel, so if you are thinking
of using a checkbox, you almost certainly want to use a ternary instead. For
example, you could create an "Organ donor" field that should be *Yes* if the
patient is an organ donor, *No* if the patient is not, and *NA* if unknown:

1.  When creating the property, set its type to *Boolean*.

2.  After creating the property, under *Custom Display*, put:

        {{include reference="PhenoTips.YesNoNAPicker" /}}

3.  When creating the user interface extension, under *Extension code*, put:

        {{include reference="PhenoTips.PatientSheetMacros" /}}

        {{velocity}}
        #if ($xcontext.action == 'edit')
          $xwiki.ssx.use('PhenoTips.YesNoNAPicker', {'colorTheme' : "$!{themeDocFullName}"})##
          $xwiki.jsx.use('PhenoTips.YesNoNAPicker')##
        #end
        #__displayIfSet('organ_donor')
        {{/velocity}}

### Lists of options

It's also possible to create a property that is a question with an arbitrary but
finite number of possible answers. The following example is for a property to
record relevant eye surgeries that the patient has had:

1.  When creating the property, set its type to *Static List*.

2.  After creating the property, fill in the *Values* field with the list of
    possible surgeries.

    To make the values recorded the same as the text displayed, simply separate
    them with pipe symbols:

        Cataract surgery|Glaucoma surgery|Refractive surgery

    To record values that are different from the text displayed, combine the
    value and the text with an equals sign. This is particularly useful for
    recording coded data:

        CPT 66982=Cataract surgery|CPT 66150=Glaucoma surgery|CPT 66840=Refractive surgery

    Do not insert spaces around the pipe symbols or equals signs; if you do,
    spaces will be inserted into the recorded values too.

3.  **If the possible values are mutually exclusive**, under *Display Type*,
    choose *select* if you want a drop-down or *radio* if you want radio
    buttons. **If the possible values are not mutually exclusive**, check
    *Multiple Select* and under *Display Form* choose *checkbox*.

4.  (Optional) By default, all of the checkboxes or radio buttons are displayed
    together on one line. To get a line break after each one, after creating the
    user interface extension, hover over the *Edit* button and click *Objects*.
    Then click the + sign next to *StyleSheetExtension 0* and put the following
    under *Code*:

        .eye_surgeries > .displayed-value > label {
          display: block;
        }

    Finally, under *Use this extension*, change *On demand only* to
    *On this wiki*.

### Instructional text

Sometimes you need to add a paragraph of instructions that apply to a set of
fields. In this case, you extend the user interface but not the underlying data
model. The following example adds a paragraph of text at the beginning of the
"Patient information" section:

1.  Click *Administration*, then click *UIX*.

2.  Under *New extension name*, enter `UIX_Field__patient_info_instructions` for
    the new user interface element and click *Create*.

3.  Under *Extension code*, put the text that you want to display, such as
    `Please make sure that all information in this section exactly matches the
    patient's EHR record in EPIC.` *Extension code* accepts
    [XWiki syntax](http://platform.xwiki.org/xwiki/bin/view/Main/XWikiSyntax)
    for bold, italic, lists, and other formatting.

4.  (Optional) Under *Extension parameters*, type
    `title=Patient information instructions`.

5.  Click *Save and view summary*.

6.  Go to *Administration* > *Patient form structure*, drag and drop the
    "Patient information instructions" item to the top of the "Patient
    information" section, and click *Save patient form configuration*.

### Arrays of classed objects

Some PhenoTips fields, such as "Measurements" and "List of candidate genes", are
actually arrays of objects attached to the patient. You can attach custom
objects to a patient by creating your own class and a user interface component
that displays an array of objects of that class. The following example creates a
class for eye examinations, where we are interested in the date of the exam and
the presence or absence of conjunctivitis:

1.  Go to /bin/create/PhenoTips/AllDocs and under *Page name* put
    `EyeExamClass`. Click *Create*, then click *Save and view summary*.

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
        #__extradata_displayTable('PhenoTips.EyeExamClass', {'counter' : false, 'labels' : true, 'mode' : $xcontext.action})
        {{/velocity}}

10. (Optional) Under *Extension parameters*, type `title=Eye exams`.

11. Click *Save and view summary*.

12. Go to *Administration* > *Patient form structure*, drag and drop the "Eye
    exams" item onto the default patient form, and click *Save patient form
    configuration*.

### Files and images

PhenoTips lets you create a property that holds a file or image as opposed to a
string, number, date, etc. There are a few things that you need to do to make
this work:

1.  When you create the property, set its type to *Static List*.

2.  After you create the property, set its *Custom Display* to

        {{include reference="PhenoTips.ImageDisplayer" /}}

    The name is misleading; use the same code for both files and images.

3.  (Optional) To allow the property to contain multiple files or images, check
    *Multiple Select*.

To get the exact same user interface that PhenoTips uses for attaching files,
the property must be named "file" and be in a
[custom class](#arrays-of-classed-objects).

Note that there are actually two steps to attaching an file to a patient:
*uploading* the file and *selecting* the file. When you upload a file for use
in one field, it will also appear as an option for use in other fields. A file
will remain in the patient's database of images, even if it is not used, until
you click *Upload and manage*, hover over the file you want to delete, click the
X in the upper-right corner, and click *Yes*. All files uploaded to a patient,
both used and unused, are deleted when the patient is deleted.

### ClinVar

The [PhenoTipsBot](https://github.com/eilbecklab/phenotipsbot) project includes
a [script to export data from PhenoTips for ClinVar submission]
(https://github.com/eilbecklab/phenotipsbot#export-clinvarpy). To support adding
variants to patients for ClinVar submission:

1.  Download
    [PhenoTips.ClinVarVariantClass.xar](Phenotips.ClinVarVariantClass.xar) and
    [PhenoTips.UIX_Field__clinvar_variants.xar](PhenoTips.UIX_Field__clinvar_variants.xar).
2.  Go to *Administration* > *Import* and import both packages.
3.  (Optional) Go to *Administration* > *Patient form structure*, drag and drop
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
