# OCR Optimization for Scanned Texts

## Problem Statement
Performing OCR on scanned texts presents challenges, especially with complex layout / double column formats. Usage of Google Vision API (at the time of writing this) leads to the columns getting mixed up, creating jumbled results. Manually fixing these errors is impractical due to the volume of inputs.

## Proposed Solution
We propose a combination of Adobe OCR and GPT3 to address this issue. The workflow is as follows:

1. **Batch Processing with Adobe:** Use Adobe software or Adobe PDF Services API to process PDF files in batches.
2. **Conversion to RTF:** The OCR-detected PDFs are then batch converted to RTF files.
3. **RTF to Plain Text:** Convert RTF files to plain text using Microsoft Word Macros.
4. **Cleaning up text using GPT3:** The plain text is cleaned up using GPT using relevant prompts.

### Prerequisites
- PDFs must be of good scan quality, with preprocessing to eliminate skew and noise if any.
- Access to Adobe PDF Services API or Adobe Pro DC subscription.
- Microsoft Word installation with Developer Options enabled.

### Limitation
- This method gives significant improvement over Google Vision for scanned pages having multiple columns or mixed layout. For simple and single column layouts, Google Vision API is still a good choice.

## Contributing
There may be alternate solutions. Contributions to enhance the process or extend its capabilities are welcome.

## License
This project is licensed under the [MIT License](LICENSE).

## Acknowledgements
Special thanks to the UiO IT team enabling access to API keys from OpenAPI.
