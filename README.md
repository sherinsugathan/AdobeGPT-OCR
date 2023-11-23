# OCR Optimization for Scanned Texts

![](https://github.com/sherinsugathan/AdobeGPT-OCR/blob/6bba2ef5376c042250e761a7a96bb1bc4673678c/testResults/snapshot.png)

## Description
Performing OCR on scanned texts presents challenges, especially with complex layout / double column formats. Usage of Google Vision API (at the time of writing this) sometimes leads to the columns getting mixed up, creating jumbled results. Manually fixing these errors is impractical especially when there is a large volume of inputs to process.
![](https://github.com/sherinsugathan/AdobeGPT-OCR/blob/2b31667f99ed68f62d78adbcd09905a7738065eb/sampleImages/ExampleDocumentLayout.png)
## Workflow
We propose a combination of Adobe OCR and GPT3 to address this issue. The workflow is as follows:

1. **Batch Processing with Adobe:** _Use Adobe software or Adobe PDF Services API to process PDF files in batches._

    a. Start Adobe Acrobat Pro and Select **Scan & OCR** from the **Tools** tab.

    b. Select **recognize text in multiple files.** 

    c. Add all files that needs to be processed or Select a folder that contain all files and Press **OK**. 
    
    d. From the Output Options popup that appears, set your target folder and file name choices and Press **    OK**.

    e. From the **Recognize Text** popup, set the language of your document, and set other desired output options.

    f. Press OK to start processing. 

_Note: Adobe will keep opening and processing files one by one. The same process can be also done programmatically using the Adobe PDF Service API._

2. **Batch Conversion to RTF:** _The OCR-detected PDFs are then batch converted to RTF files._

    a. Select the **Action Wizard** tool from the **Tools** tab in Adobe Acrobat.

   b. Select **New Action**.

   c. Under the **Create New Action** window, Select **Save & Export** > **Save** from the left pane and add it to the right-side pane.

   d. Using the pane on the right side, set input folder location (containing OCR-detected PDFs) or input files.

   e. On the right-side pane, Click **Specify Setting** under the Save action.

   f. From the **Output Options** window, select **Export File(s) to Alternate Format** and choose "**Rich Text Format**" as the format. 

   g. Click **Save** and provide any suitable name for the Action.

   h. Click on the newly created action from the **Actions List**.

   i. Add files/folder to process and press **Start**.

_Note: Adobe will keep opening and processing files one by one. The same process can be also done programmatically using the Adobe PDF Service API._

3. **Batch Convert RTF to Plain Text:** This step batch converts RTF files to plain text using Microsoft Word Macros.

   ***I. Enabling Developer Options in Word***

   a. Go to **File** > **Options**.

   b. In the **Word Options** dialog, select **Customize** Ribbon.

   c. In the right pane, check the **Developer** checkbox.

   d. Click **OK**.

   ***II. Using Macros for Batch Conversion***

   a. Go to the **Developer** tab.

   b. Click on **Visual Basic** to open the VBA editor.

   c. In the VBA editor, right-click on **ThisWorkbook** in the Project Explorer.

   d. Choose **Insert** > **Module**. This creates a new module for your macro code.

   e. Copy and paste the following VBA code into the module:

```angular2html
Sub ConvertRTFtoTXT()

    Dim sourceFolder As String, destFolder As String
    Dim docFile As String
    Dim doc As Document
    
    ' Specify the source folder containing the RTF files
    sourceFolder = "C:\Sherin\Workspace\2_Datasets\OCR_dataset\AnalysisForKnut\OCRcjeu_texts-main\data_derived\combined\OCRed\RTF\"
    
    ' Specify the destination folder where you want to save the TXT files
    destFolder = "C:\Sherin\Workspace\2_Datasets\OCR_dataset\AnalysisForKnut\OCRcjeu_texts-main\data_derived\combined\OCRed\RTF\TXT\"
    
    ' Ensure the folder paths end with a backslash
    If Right(sourceFolder, 1) <> "\" Then sourceFolder = sourceFolder & "\"
    If Right(destFolder, 1) <> "\" Then destFolder = destFolder & "\"
    
    ' files
    docFile = Dir(sourceFolder & "*.rtf")
    
    ' Loop through each RTF file in the source folder
    Do While docFile <> ""
    
        ' Open the RTF file
        Set doc = Documents.Open(sourceFolder & docFile)
        
        ' Save it as a plain text file in the destination folder
        doc.SaveAs2 destFolder & Replace(docFile, ".rtf", ".txt"), wdFormatText, Encoding:=msoEncodingUTF8
        
        ' Close the RTF file
        doc.Close
        
        ' Get the next RTF file from the source folder
        docFile = Dir
    Loop

End Sub
```

4. **Cleaning up text using GPT3:** The plain text is cleaned up using GPT using relevant prompts.

### Prerequisites
- PDFs must be of good scan quality, with preprocessing to eliminate skew and noise if any.
- Access to Adobe PDF Services API or Adobe Pro DC subscription.
- Microsoft Word installation with Developer Options enabled.

### Results


### Limitations
- This method gives significant improvement over Google Vision for scanned pages having multiple columns or mixed layout. For simple and single column layouts, Google Vision API is still a good choice.
- Adobe's text recognition is not as good as Google Vision (but layout reading order is better than Google Vision). We use GPT to minimize the spelling errors in the text.

## Contributing
There may be alternate solutions. Contributions to enhance the process or extend its capabilities are welcome.

## License
This project is licensed under the [MIT License](LICENSE).
