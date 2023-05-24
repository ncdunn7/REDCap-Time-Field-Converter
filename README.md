# REDCap Time Field Converter
A guide to setting up a REDCap field that converts a military time (HH:MM) validated text field to 12-hour am/pm time in real time so that participants can get instant feedback and adjust their responses appropriately while maintaining strict time validation.

_\*Please note that at the time of making this SOP, the current REDCap version is 13.1.25. Future versions of REDCap may make this solution obsolete._

**Introduction**

REDCap field validation does not have an option to validate time fields for AM/PM times. To work around this, the following logic will allow you to provide the participant with a separate field that has a real time conversion of a 24-hour clock to a 12-hour clock time. For this example, we will need 3 fields/variables for each item you would like a conversion for:

1. **Primary Variable:** a text box field for the original item, validated in the time format of HH:MM.
  - In this example, I will be naming this variable, **var**
2. **Hour Variable:** a calculated field that takes the 2 leftmost characters in **var** (i.e., HH), checks whether the first character is a 0. If the leading number is 0, it transforms the second character in the **var** text string (i.e., H **H** ) and turns it into a number. If the leading number is not a 0, it transforms the first 2 characters in **var** (i.e., **HH** ) into a number.
  - In this example, I will be naming this variable, **var\_h**
3. **Conversion Variable:** an unvalidated text box field where the time is converted to a 12-hour clock.
  - In this example, I will be naming this variable, **var\_ampm**

**Instructions**

1. First, if you have not created your Primary Variable and Field ( **var** ), create it now. Make sure to set the validation as **HH:MM**.

![](RackMultipart20230524-1-e0qjh0_html_84ffcd355785264.png)

1. Now, let's add our Hour Variable and Field ( **var\_h** ):
  - Set the field type to **calculated field**.
  - In the **Calculation Equation** box, you will be copy/pasting your first logic block:

```
if(left([var],1)="0",mid([var],2,1),left([var],2))
```

- ![](RackMultipart20230524-1-e0qjh0_html_7c4c12a61adff512.png)If you did everything correctly, your **var\_h** variable should look like this:

1. Finally, let's create our Conversion Variable ( **var\_ampm** )
  - Set the field type to **Text Box**
  - In the **Action Tags/Field Annotation** box, you will be copy/pasting the following logic:

```
@CALCTEXT(if([var\_h]\<\>0 AND [var\_h]\< 10,concat(mid([var],2,1),":",right([var],2)," ","AM"),if([var\_h]=10,concat("10",":",right([var],2)," ","AM"),if([var\_h]=11,concat("11",":",right([var],2)," ","AM"),if([var\_h]=0,concat("12",":",right([var],2)," ","AM"),if([var\_h]=12,concat("12",":",right([var],2)," ","PM"),if([var\_h]=13,concat("1",":",right([var],2)," ","PM"),if([var\_h]=14,concat("2",":",right([var],2)," ","PM"),if([var\_h]=15,concat("3",":",right([var],2)," ","PM"),if([var\_h]=16,concat("4",":",right([var],2)," ","PM"),if([var\_h]=17,concat("5",":",right([var],2)," ","PM"),if([var\_h]=18,concat("6",":",right([var],2)," ","PM"),if([var\_h]=19,concat("7",":",right([var],2)," ","PM"),if([var\_h]=20,concat("8",":",right([var],2)," ","PM"),if([var\_h]=21,concat("9",":",right([var],2)," ","PM"),if([var\_h]=22,concat("10",":",right([var],2)," ","PM"),if([var\_h]=23,concat("11",":",right([var],2)," ","PM"),if([var\_h]=24,concat("12",":",right([var],2)," ","AM"),""))))))))))))))))))
```

- If you did everything correctly, your Conversion Variable should look like this:

![](RackMultipart20230524-1-e0qjh0_html_9a5c9da042432cf4.png)

1. With those 3 variables complete, we can test now to make sure the conversion is working. Let's add a new record and type a time into the Primary Variable field:

![](RackMultipart20230524-1-e0qjh0_html_bcd35ab55359d8b6.png)

1. Success! Now, let's clean up our variables so that it only the Primary Variable and Conversion Variable are visible.
  - Return to Designer
  - Edit the Hour Variable, adding the action tag @HIDDEN
  - We should no longer be able to see the Hour Variable

Now you have the tools to set this conversion up in any REDCap project. If you are wondering how you might change the variable names to match your survey items, we can easily replace the variable names in the code using Word:

1. Copy the code blocks from above into a **new Word Document**
2. In the top right of the **home ribbon** , you should see a button called **Replace**. Go ahead and open that menu.
3. In the **Replace** menu…
  - Type " **var**" into the **find what** text box.
  - Type **your Primary Variable name** into the **replace with** text box.
  - Click the **Replace all** button at the bottom.

![](RackMultipart20230524-1-e0qjh0_html_8766bb61f8259823.png)

1. All of the **var** variables should have been replaced with your primary variable name. Rinse and repeat for other variables.
2. You can now copy/paste that code directly into REDCap!
