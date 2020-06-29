*This is not the final version of the application

## Prerequisite
**Note: The below steps are only required if you want to extract data from scanned pdf. If there are only vector PDFs no need to do the following steps**

1. Download Tesseract. Follow this link: [Download Tesseract](https://github.com/UB-Mannheim/tesseract/wiki)

2. Add the path of Tesseract OCR folder (e.g ```C:\Program Files\Tesseract-OCR```) to the Environmental Variables -> System Variables -> Path. 

   Follow this: Control panel -> System and Security -> System -> Advanced system settings -> Enironmental Variables -> Select 'Path'    (in lower section named System variables) and click 'Edit'-> New -> add path to the folder Tesseract-OCR. Check in your file explorer where this folder is: most likely this ```C:\Program Files\Tesseract-OCR```

3. Download ImageMagick. Follow this link: [Download ImageMagick](https://legacy.imagemagick.org/script/binary-releases.php)

4. Add the path of ImageMagick folder (e.g ```C:\Program Files\ImageMagick-6.9.11-Q16```) to the Environmental Variables -> System Variables -> Path. Same as step 3

5. Go to ImageMagick folder (e.g ```C:\Program Files\ImageMagick-6.9.11-Q16```) and find the *convert.exe* application. Rename it from '*convert*' to '*imconvert*'. Reason for it is windows already contain another *convert.exe* application therefore there is a possibility of overriding our imagemagick *convert.exe*


## Installion Guide
**Only for 64-bit currently**

1. Download [this](https://drive.google.com/file/d/1DylXqRec8tYq-Av6SJTzHR7Ib9jeqN4L/view?usp=sharing) zip file and extract it somewhere

2. The zip contains four items 
   - main.exe
   - pdftotext.exe
   - folder containing PDFs
   - folder containing templates
   
**Note: Don't delete pdftotext.exe. Also keep the pdftotext.exe and main.exe in the same directory**
   
3. Run main.exe follow these steps
   - Add the template folder containg templates files in ```.yml``` format. (You can add your own template folder)
   ![Step 1](https://github.com/ssj-ali/pdfextract/blob/master/Screenshot%20(37).png?raw=true)
   ![Step 2](https://github.com/ssj-ali/pdfextract/blob/master/Screenshot%20(39).png?raw=true)
   - Add multiple .pdf files to the listbox and press Done
   ![Step 3](https://github.com/ssj-ali/pdfextract/blob/master/Screenshot%20(40).png?raw=true)
   ![Step 4](https://github.com/ssj-ali/pdfextract/blob/master/Screenshot%20(41).png?raw=true)
   ![Step 5](https://github.com/ssj-ali/pdfextract/blob/master/Screenshot%20(42).png?raw=true)
   ![Step 6](https://github.com/ssj-ali/pdfextract/blob/master/Screenshot%20(44).png?raw=true)
   ![Step 7](https://github.com/ssj-ali/pdfextract/blob/master/Screenshot%20(46).png?raw=true)
 
**__ Note:Extract Text button is used while creating template. See [Template Tutorial](https://github.com/ssj-ali/pdfextract/blob/master/TUTORIAL.rst)__**
   

## Key Points

1. If a PDF is a vector pdf, pdftotext is used to extract data from pdf and it works instantly (< 1 sec per PDF)

2. If a PDF is a scanned pdf, tesseract + imagemagick + ghostscript is used to extract data. (Time: 4 sec per page)



## Template System

1. The extracted fields only depends on template i.e if more fields are required to be extracted, we just need to edit templates.

2. In templates, the field name should be same for all pdfs. For example, if the field name for serial number is SerialNo in one template and S.No in other template then both will considered different field in the final excel sheet.

3. We can have multiple regex per field (if layout or wording changes)

4. For creating a template see [Template Tutorial](https://github.com/ssj-ali/pdfextract/blob/master/TUTORIAL.rst)



## Challenges

1. Very few times OCR is unable to detect properly because the scanned PDF was not clean. Errors are like 0(zero) is scanned as O(letter)
