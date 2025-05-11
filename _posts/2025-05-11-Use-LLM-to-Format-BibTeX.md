---
title: "How I used the LLM to streamline my BibTeX formatting"
categories:
- Coding
tags:
- artificial intelligence
- solution
date: 2025-05-11
layout: post
---

## The problem definition

I have a [webpage](https://mengxiangxi.info/Link/uexplorer.html) to keep track of all peer-reviewed papers I could find on the world's first total-body PET scanner, uEXPLORER. On this webpage, I used a javascript code called [bibtex_js](https://github.com/pcooksey/bibtex-js) to render the bibliography from a bibtex file that I manually maintain. Over the years, I have developed a set of rules and conventions to build the bibliography entries, assigning different meanings and functions to different fields.

For a certain paper that I want to include in my bibliography, [like this one](https://link.springer.com/article/10.1007/s40291-025-00778-6), I will add the following entry to my bibtex file:

```bibtex
@article{filippi2025fapitargeted,
title={FAPI-targeted molecular imaging: Transforming insights into post-ischemic myocardial remodeling?},
author={Filippi, Luca and Perrone, Marco Alfonso and Schillaci, Orazio},
journal={Molecular Diagnostics \& Therapy},
year={Just Accepted},
doi={10.1007/s40291-025-00778-6},
pmid={40263181},
institution={Tor Vergata University Hospital},
keywords={FAPI; cardiovascular}
}
```

Then, the javascript code will render the entry as follows:

> **FAPI-targeted molecular imaging: Transforming insights into post-ischemic myocardial remodeling?**
> Luca Filippi, Marco Alfonso  Perrone, Orazio Schillaci. *Molecular Diagnostics \& Therapy*
> [[doi link]](https://dx.doi.org/10.1007/s40291-025-00778-6) [[PubMed]](https://pubmed.ncbi.nlm.nih.gov/40263181/) | Tor Vergata University Hospital | **Tag:** FAPI; cardiovascular

## Manual Workflow

In my routine workflow, I usually first export a preliminary bibtex entry using the Google Scholar [button](https://chromewebstore.google.com/detail/ldipcbpaocekfooobnbcddclnhejkcpn) browser addon. Then, I retrieve various information from the publisher's site and [PubMed](https://pubmed.ncbi.nlm.nih.gov/), gradually filling out all fields.

The journal title should be in the Sentence case; the author names should not be abbreviated; the institution names should be concise and consistent; the keywords should be reflect the tracers, the diseases, and the type of paper. Multiple rules apply which requires complicated analysis.

Admittedly, this is slow and distractive. My original intention was to keep a record of the papers I have read, but I found myself spending more time on formatting than on reading.

## How AI helps

Things changed when I realized that I could build an agent using some certain MCPs (model context protocol). There is one called "fetch" which allows the LLM to retrieve information from a webpage. There is another called "PubMed MCP" which can communicate with the PubMed website. Those are almost all I need.

Thus, I first wrote my own version of rules and steps as a prompt. I asked Gemini to generated lists of the known keywords and institutions from [my existing bibtex file](https://mengxiangxi.info/bibliography/uExplorer.bib). My human-intelligence version of the prompt is as follows:

```text
I am going to provide you a link to a certain web page, or other sources indicating a specific paper. Your task is to generate the bibtex style citation entry of this article. The entry should include the title, author, journal, year, volume, number, pages, doi, pmid, institution, and keywords.

- Please closely follow the styles in the final "=== Example BibTeX File===" section (the example file).
- The citekey should consist the last name of the author+ year + two to three key words in the title
- If the paper has not been assigned the final page number (such as early access), the "year" should be "Just Accepted".
- If an article is determined as "Just Accepted", you should not leave blank "pages", "number", or "volume" filed, rather, you should remove these entries. However, "doi", "pmid", and "institution" fields should always be there even if you failed to determine their values.
- The keywords has to be within the ones already contained in the example webpage. Do not add new words unless absolutely necessary. Hint: You can consult the list of all keywords and all institutions names provided in the "=== Lists ===" section before proceeding.
- The keywords should contain the tracer name (EXCEPT "FDG"), the related disease (if clearly defined), and the related areas, among others. It should NOT contain too general terms such as "total body", "PET".
- The journal names should be spelled in the full format. The Journal Name Should be Capitalized.
- Pay attention to the superscript and subscript, use html elements to wrap around them. The title should be in the sentence case.
- Pay attention to the author names, names that are already contained in the example webpage should have the same format. The given names should not be abbreviated to only one letter. For example, "Given, F." should be "Given, First". However, middle names can be abbreviated when necessary, in which case, omit the period.
- You can use a tool to look up pubmed for the pmid when necessary.
- Do not use leading spaces in formatting. Use as few spaces as possible. Avoid any tabs.
- If there are multiple institutions, or keywords, connect them with a semicolon.

STEPS:

1. Fully appreciate the rules, understand all requirements and exceptions.
2. Retrieve the information from the article link provided. Determine the title, authors (full name), journal, page, number volume, read throught the abstract. Hint: directly fetch the url.
3. Generate the citekey.
4. Format the title in sentence case and make sure the subscripts and superscripts are taken care of.
5. Generate the author list, check if the Given Names are complete. Check if the format of the authors are consistent with the names already in the example bib file. Double check the author list with the source, to make sure no errors are introduced during formatting.
6. Format the journal name.
7. Check if it a "Just Accepted", if so, remove page, number, and volume.
8. Generate doi and pmid.
9.  Analyze the institutions, format them according to the institutions in the example file. Do not include the university if the instution is an affiliated hospital.
10. Choose and format keywords. No need to capitalize the first letter in the keywords.

=== Special Considerations ===

Finally, in the following situations, the following rules have higher priority than other rules:

- The journal name "Journal of Nuclear Medicine" should be "J. Nucl. Med."
- The following institution names should be changed:
{"The First Affiliated Hospital of Shandong First Medical University" should be "Qianfoshan Hospital";}

=== Lists ===

Institutions:
Guangdong People's Hospital; Nanfang Hospital; Ruijin Hospital; University of Bern; 
### more entries omitted ###

Keywords:
novel tracer; lung cancer; immune; PD-L1; [68Ga]Ga-DOTA-PEG2-Asp2-PDL1P; PDL1P; ### more entries omitted ###

=== Example BibTeX File===

@article{zhang2024myocardialmetabolism,
title={Feasibility of shortening scan duration of <sup>18</sup>F-FDG myocardial metabolism imaging using a total-body PET/CT scanner},
author={Zhang, Xiaochun and Xiang, Zeyin and Wang, Fanghu and Han, Chunlei and Zhang, Qing and Liu, Entao and Yuan, Hui and Jiang, Lei},
journal={EJNMMI Physics},
year={Just Accepted},
doi={10.1186/s40658-024-00689-1},
pmid={39390229},
institution={Guangdong People's Hospital},
category={#Clinical},
keywords={fast; cardiovascular}
}

### more examples omitted ###

```

At this point, I realized that I have another agent specifically designed to optimize the prompts. Thus, I asked this "prompt engineer" to help me optimize the prompt for this task. It suddenly became more logical and clear. After some manual editing, the optimized version looks like this:

```text
I want you to generate properly formatted BibTeX citation entries for academic articles based on the links or sources I provide. Each entry must comply fully with the following rules and exceptions. Please follow these steps in order to deliver accurate and consistent results:  

---

## **General Rules for Generating BibTeX Entries**  

### Tools to use

- Use a "fetch_txt" tool to retrieve the full citation details from the provided link.
- Use a "pubmed" tool only if necessary (such as to determine the PMID).

### Formatting Guidelines

1. **Required Citation Components**:  
   - Include the following fields: `title`, `author`, `journal`, `year`, `volume`, `number`, `pages`, `doi`, `pmid`, `institution`, and `keywords`.  
   - If the paper has no final page numbers and is categorized as "Just Accepted," remove the `volume`, `number`, and `pages` fields; however, always include `doi`, `pmid`, and `institution`.

2. **Citekey**:  
   - Format: `<last name of first author><year><two-to-three meaningful keywords from the title>`.  
   - Example: If the first author’s last name is "Zhang," the year is "2024," and the title includes "myocardial metabolism imaging," the citekey should be `zhang2024myocardialmetabolism`.

3. **Title Formatting**:  
   - Titles should use **sentence case** (capitalize only the first word and proper nouns).  
   - Handle any superscripts (e.g., `<sup>18</sup>F`) and subscripts (e.g., `H<sub>2</sub>O`) using HTML tags.

4. **Author Names**:  
   - Ensure full given names are used, unless they are already standardized in the example file (e.g., "Gu, First" instead of "Gu, F."), but middle names may use initials (no period after the initial).
   - Cross-check author formats with names in the example BibTeX for consistency.  

5. **Journal Name**:  
   - Use full journal names in title case unless otherwise specified. For example "Physics in Medicine \& Biology".

6. **Institution Names**:  
   - Follow the standard institution list provided in the "=== Lists ===" section.
   - Identify only the most important institution.
   - Keep the institution name succinct.
   - Omit universities if the institution is an affiliated hospital.

7. **Keywords**:  
   - Choose keywords from the predefined list in the "=== Lists ===" section.  
   - Include terms that describe the tracer used (except "FDG"), related diseases (if specified), and relevant specialties. Avoid overly general terms like "total body" or "PET."  
   - Connect multiple keywords with semicolons.
   - For new tracers, use the standard EANM-recommended tracer name as the keyword (ignore superscript or subscript). If applicable, also include the family of the tracer, such as PSMA, DOTATATE, FAPI, etc. For tracers that are not widely used, use the keyword "novel tracer".

8. **Handling "Just Accepted" Articles**:  
   - For "Just Accepted" papers with no final volume, number, or page numbers: 
     - The value of the tag "year" should be "Just Accepted".
     - **Remove** fields for `volume`, `number`, and `pages`.  
     - Always include `doi`, `pmid`, and `institution`, even if incomplete.

9. **BibTeX style**:  
   - Do not use leading spaces in formatting. Use as few spaces as possible. Avoid any tabs.

---

### **Step-by-Step Process**  

1. **Understand the Guidelines**:  
   - Fully review all general rules, special considerations, and exceptions before starting.  

2. **Retrieve the Information**:  
   - Fetch the article from the provided link using a tool and gather all necessary citation information, such as title, authors, journal, year, volume, number, pages, DOI, PubMed ID (PMID), institutions, and keywords. Directly fetch from the url using a tool.

3. **Determine the Citekey**:  
   - Create the citekey based on the rules (last name + year + keywords).  

4. **Format the Title**:  
   - Convert the title into **sentence case**. Insert `<sup>` and `<sub>` HTML tags for any superscripts or subscripts.  

5. **Prepare the Author List**:  
   - Ensure that author names are in the correct format. Verify that the given names are not abbreviated (e.g., "First name" instead of "F.").

6. **Format the Journal Name**:  
   - Use the full journal name unless exceptions (like "J. Nucl. Med.") are specified in the rules.  

7. **Check "Just Accepted" Status**:  
   - If an article is not yet published and is marked as "Just Accepted," remove the `volume`, `number`, and `pages` fields.  

8. **Complete DOI and PMID Fields**:  
   - Fetch and include the DOI and PMID values. These fields can be blank if failed to fetch, but must never be omitted.
   - You may need to use a tool to search pubmed if some information is missing.

9. **Format Institution Names**:  
   - Match the institution names to the predefined list in the "=== Lists ===" section. If multiple institutions exist, join them with semicolons.
   - New names can be added, in a consistent format as those in the list.

10. **Choose Suitable Keywords**:  
    - Select relevant keywords from the predefined list. Focus on the tracer, disease, and specialties. Avoid general terms like "total body."

11. **Compile the BibTeX Entry**:  
    - Input data into a BibTeX entry following the example format provided, applying all the rules.

12. **Final Validation**:  
    - Double-check components for accuracy, consistency with the rules, and adherence to journal formats. Correct any inconsistencies.

---

### **Special Considerations**  

- Abbreviations: Use "J. Nucl. Med." for "Journal of Nuclear Medicine." Apart from this, use full journal names.
- Redefinitions: Replace specific institution names as listed in the "Special Considerations" section. Examples include:  
  - "The First Affiliated Hospital of Shandong First Medical University" → "Qianfoshan Hospital."  

---

### **Expected Output Example**  

Here’s an example of the BibTeX output, based on the rules:  
```bibtex  
@article{zhang2024myocardialmetabolism,
title={Feasibility of shortening scan duration of <sup>18</sup>F-FDG myocardial metabolism imaging using a total-body PET/CT scanner},
author={Zhang, Xiaochun and Xiang, Zeyin and Wang, Fanghu and Han, Chunlei and Zhang, Qing and Liu, Entao and Yuan, Hui and Jiang, Lei},
journal={EJNMMI Physics},
year={Just Accepted},
doi={10.1186/s40658-024-00689-1},
pmid={39390229},
institution={Guangdong People's Hospital},
keywords={fast; cardiovascular}
}

### more examples omitted ###

=== Lists ===

Institutions:
Guangdong People's Hospital; Nanfang Hospital; Ruijin Hospital; University of Bern; 
### more entries omitted ###

Keywords:
novel tracer; lung cancer; immune; PD-L1; [68Ga]Ga-DOTA-PEG2-Asp2-PDL1P; PDL1P; 
### more entries omitted ###
```

This is about 3000 tokens for OpenAI or Gemini models, so it is still feasible. I choose to use `gemini-2.5-pro-exp` for this task.

When I send the link "https://pubmed.ncbi.nlm.nih.gov/40208315/" to the model, it responds as follows:

> I will fetch the content from the provided PubMed link.
> ```html
> <tool_use>
>   <name>fqw-KIEMo_ltGFDxcHkHAc</name>
>   <arguments>
>    {"url": "https://pubmed.ncbi.nlm.nih.gov/40208315/"}
>   </arguments>
> </tool_use>
> ```
>
> Okay, I have the text content from the PubMed link for the second article. Now I will extract the necessary information and format the BibTeX entry.
>
>Here's what I've gathered:
>*   **Title:** Al<sup>18</sup>F-NOTA-HER2-BCH versus <sup>18</sup>F-FDG PET/CT in evaluating >newly diagnosed HER2-low breast cancer patients
>*   **Authors:** Xiaoyi Guo, Xu Liang, Ben Li, Yan Mao, Nina Zhou, Jiayue Liu, Guangjie >Yang, Zhenguang Wang, Guohong Song, Zhi Yang
>*   **Journal:** European Journal of Nuclear Medicine and Molecular Imaging
>*   **Year:** 2025 (Online ahead of print, so it will be "Just Accepted")
>*   **DOI:** 10.1007/s00259-025-07251-w
>*   **PMID:** 40208315
>*   **Affiliations (First author's primary and most important):**
>    *   Xiaoyi Guo: Key laboratory of Carcinogenesis and Translational Research (Ministry of >Education), Beijing Key Laboratory of Research, Investigation and Evaluation of >Radiopharmaceuticals, NMPA Key Laboratory for Research and Evaluation of >Radiopharmaceuticals (National Medical Products Administration), Department of Nuclear >Medicine, Peking University Cancer Hospital & Institute, Beijing, China.
>*   **Keywords (from text):** <sup>18</sup>F-FDG; Al<sup>18</sup>F-NOTA-HER2-BCH; Breast >cancer; HER2-low; PET/CT.
>
>Now, I will process this information according to the rules:
>
>1.  **Citekey:** `guo2025her2bch` (First author: Guo, Year: 2025 (will be Just Accepted), >Keywords: HER2-BCH from Al<sup>18</sup>F-NOTA-HER2-BCH)
>2.  **Title Formatting:** "Al<sup>18</sup>F-NOTA-HER2-BCH versus <sup>18</sup>F-FDG PET/CT >in evaluating newly diagnosed HER2-low breast cancer patients" (Sentence case, preserving >superscripts)
>3.  **Author Names:** Guo, Xiaoyi; Liang, Xu; Li, Ben; Mao, Yan; Zhou, Nina; Liu, Jiayue; >Yang, Guangjie; Wang, Zhenguang; Song, Guohong; Yang, Zhi (Full given names, middle initial >without period).
>4.  **Journal Name:** European Journal of Nuclear Medicine and Molecular Imaging (Full name)
>5.  **Year:** Just Accepted (Since it's "Online ahead of print" and no volume/number/pages >are listed yet)
>6.  **Institution:** The primary institution for the first author is "Peking University >Cancer Hospital & Institute". This will be simplified to "Beijing Cancer Hospital" from the >provided list.
>7.  **Keywords:**
>    *   Tracer: "Al18F-NOTA-HER2-BCH" (new tracer, use EANM name, ignore superscript).
>    *   Disease: "breast cancer".
>    *   Specialty: The tracer is HER2-targeted, so "HER2".
>    *   Combined: "Al18F-NOTA-HER2-BCH; breast cancer; HER2"
>8.  **"Just Accepted" Handling**: `volume`, `number`, and `pages` fields will be removed.
>
>Now, I will compile the BibTeX entry.
>```bibtex
>@article{guo2025her2bch,
>title={Al<sup>18</sup>F-NOTA-HER2-BCH versus <sup>18</sup>F-FDG PET/CT in evaluating newly >diagnosed HER2-low breast cancer patients},
>author={Guo, Xiaoyi and Liang, Xu and Li, Ben and Mao, Yan and Zhou, Nina and Liu, Jiayue >and Yang, Guangjie and Wang, Zhenguang and Song, Guohong and Yang, Zhi},
>journal={European Journal of Nuclear Medicine and Molecular Imaging},
>year={Just Accepted},
>doi={10.1007/s00259-025-07251-w},
>pmid={40208315},
>institution={Beijing Cancer Hospital},
>keywords={Al18F-NOTA-HER2-BCH; breast cancer; HER2}
>}
>```
>

It is almost perfect. The only changes I have made is to add the other institution (Affiliated Hospital of Qingdao University), and revised tracer name in the keyword (Al18F-NOTA-HER2-BCH). This marks a huge improvement. The workflow has been greatly simplified, and the efficiency has been greatly improved. I am very satisfied.

## Appendix

Here are the prompt I am using to update the "Just Accepted" entries after the corresponding paper has been published.

```text
I am going to give you a bibtex entry. In the entry I am providing, the year tag is "Just Accepted". Your task is, based on the information in the bibtex entry, you need to determine if the paper has already been published. If the paper has still not been published (i.e. still is in "early access" or related status, without a final page number), just tell me so. Otherwise, if the paper has already been published, you need to give me a complete bib entry, with year, volume, number and pages updated, and all other fields identical to the input.

You should not change the format in the output. Do not add spaces.

Example input:
@article{gu2024lowbmi,
title={Low dose optimization for total-body 2-[<sup>18</sup>F]FDG PET/CT imaging: a single-center study on feasibility based on body mass index stratification},
author={Gu, Taoying and Liu, Siwei and Hou, Xiaoguang and Zhao, Liwei and Ng, Yee Ling and Wang, Jingyi and Shi, Hongcheng},
journal={European Radiology},
year={Just Accepted},
doi={10.1007/s00330-024-11039-1},
pmid={39214892},
institution={Zhongshan Hospital},
keywords={low dose}
}
===
Output:
@article{gu2024lowbmi,
title={Low dose optimization for total-body 2-[<sup>18</sup>F]FDG PET/CT imaging: a single-center study on feasibility based on body mass index stratification},
author={Gu, Taoying and Liu, Siwei and Hou, Xiaoguang and Zhao, Liwei and Ng, Yee Ling and Wang, Jingyi and Shi, Hongcheng},
journal={European Radiology},
year={2025},
volume={35},
number={4},
pages={1881--1893},
doi={10.1007/s00330-024-11039-1},
pmid={39214892},
institution={Zhongshan Hospital},
keywords={low dose}
}
```

And this one is used to update the lists of all authors (not included in the main prompt due to the extra length/tokens), all institutions and all keywords.

```text
I want you to generate three lists from the following url (a bibtex file): https://mengxiangxi.info/bibliography/uExplorer.bib
The first list would be a list of all author names;
The second list would be a list of all institution names;
The third list would be a list of all keywords.

Each list should be in one line. Please remove the duplicated entries. The different entries should be separated by a semicolon.
```
