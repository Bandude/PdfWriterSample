# Populate PDF Form Fields using PdfSharpCore

This sample demonstrates how to take input form data and populate it to a PDF template with form fields using the `PdfSharpCore` library in C#.

## Prerequisites

- .NET SDK 6.0 or later

## Installation

1. Install the `PdfSharpCore` NuGet package:

```sh
dotnet add package PdfSharpCore
```

2. Add the following using directives to your C# file:

```csharp
using System.IO;
using PdfSharp.Pdf;
using PdfSharp.Pdf.IO;
using PdfSharp.Pdf.AcroForms;
```

## Usage

1. Create a method to read the PDF template:

```csharp
public static PdfDocument GetPdfTemplate(string templatePath)
{
    PdfDocument pdfDocument = PdfReader.Open(templatePath, PdfDocumentOpenMode.Modify);
    return pdfDocument;
}
```

2. Create a method to fill the PDF form fields using the input data:

```csharp
public static void FillPdfFormFields(PdfDocument pdfDocument, Dictionary<string, string> formData)
{
    PdfAcroForm pdfAcroForm = pdfDocument.AcroForm;
    if (pdfAcroForm.Elements.ContainsKey("/NeedAppearances"))
    {
        pdfAcroForm.Elements["/NeedAppearances"] = new PdfBoolean(true);
    }
    else
    {
        pdfAcroForm.Elements.Add("/NeedAppearances", new PdfBoolean(true));
    }

    foreach (var data in formData)
    {
        PdfTextField pdfFormField = (PdfTextField)(pdfAcroForm.Fields[data.Key]);
        if (pdfFormField != null)
        {
            pdfFormField.Value = new PdfString(data.Value);
        }
    }
}
```

3. Create a method to save the filled PDF:

```csharp
public static void SaveFilledPdf(PdfDocument pdfDocument, string outputPath)
{
    pdfDocument.Save(outputPath);
    pdfDocument.Close();
}
```

4. Create a method to process the input form data and generate a filled PDF:

```csharp
public static void GenerateFilledPdf(string templatePath, string outputPath, Dictionary<string, string> formData)
{
    PdfDocument pdfDocument = GetPdfTemplate(templatePath);
    FillPdfFormFields(pdfDocument, formData);
    SaveFilledPdf(pdfDocument, outputPath);
}
```

5. Now, you can use the `GenerateFilledPdf` method to create a filled PDF using your input form data:

```csharp
public static void Main(string[] args)
{
    string templatePath = "path/to/your/pdf/template.pdf";
    string outputPath = "path/to/save/filled/pdf/filled_template.pdf";

    // Replace these keys with your PDF form field names
    Dictionary<string, string> formData = new Dictionary<string, string>
    {
        { "FirstName", "John" },
        { "LastName", "Doe" },
        { "Email", "john.doe@example.com" }
    };

    GenerateFilledPdf(templatePath, outputPath, formData);
}
```

6. Run your application, and it should generate a filled PDF using the provided input form data.

Make sure to replace the `templatePath`, `outputPath`, and `formData` with appropriate values based on your PDF template and input form data.
