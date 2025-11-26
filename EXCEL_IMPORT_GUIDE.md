# Excel Import Guide

This guide explains how to import questions from Excel files (.xlsx or .xls) into the quiz system.

## Installation

Make sure you have the required library installed:

```bash
pip install openpyxl
# OR
pip install pandas
```

Both libraries are already in `requirements.txt`, so if you've installed dependencies, you're good to go!

## Usage

### Basic Command

Place your Excel files in the `inbox` folder and run:

```bash
python manage.py import_questions_excel
```

### Advanced Options

```bash
python manage.py import_questions_excel \
    --inbox-folder inbox \
    --sheet-name "Sheet1" \
    --header-row 1 \
    --start-row 2
```

**Options:**
- `--inbox-folder`: Folder containing Excel files (default: `inbox`)
- `--sheet-name`: Specific sheet name to import (default: first sheet)
- `--header-row`: Row number containing column headers (default: 1)
- `--start-row`: Row number where data starts (default: 2)

## Excel File Format

### Required Columns

Your Excel file must have these columns (case-insensitive):

1. **Exam** - Name of the exam
2. **Question Text** - The question content
3. **Choice 1, Choice 2, Choice 3, Choice 4** - Answer options (at least one choice required)
4. **Correct Answer** - The correct choice (exact text or number like "1", "2", etc.)

### Optional Columns

- **Category** - Category name (default: "General")
- **Topic** - Topic name
- **Explanation** - Explanation of the answer
- **Difficulty** - `easy`, `medium`, or `hard` (default: `medium`)
- **Points** - Points for the question (default: 1)
- **Question Type** - `single`, `multiple`, or `true_false` (default: `single`)

### Column Name Variations

The import script accepts various column name formats (case-insensitive):

- **Exam**: `Exam`, `exam`, `EXAM`
- **Question Text**: `Question Text`, `question_text`, `Question`, `question`
- **Choice 1**: `Choice 1`, `choice_1`, `Choice1`, `Option 1`, `option_1`
- **Correct Answer**: `Correct Answer`, `correct_answer`, `Answer`, `answer`
- **Category**: `Category`, `category`, `Cat`, `cat`
- **Difficulty**: `Difficulty`, `difficulty`, `Diff`, `diff`
- **Topic**: `Topic`, `topic`
- **Explanation**: `Explanation`, `explanation`, `Expl`, `expl`
- **Points**: `Points`, `points`, `Point`, `point`
- **Question Type**: `Question Type`, `question_type`, `Type`, `type`

## Example Excel Structure

| Category | Exam | Question Text | Choice 1 | Choice 2 | Choice 3 | Choice 4 | Correct Answer | Explanation | Difficulty | Topic | Points | Question Type |
|----------|------|---------------|----------|----------|----------|----------|----------------|-------------|------------|-------|--------|---------------|
| Math | Basic Math Test | What is 2 + 2? | 3 | 4 | 5 | 6 | 4 | Simple addition: 2 + 2 = 4 | easy | Arithmetic | 1 | single |
| Science | Science Quiz | What is H2O? | Water | Oxygen | Hydrogen | Carbon Dioxide | Water | H2O is the chemical formula for water | easy | Chemistry | 1 | single |
| History | World History | Who was the first President of India? | Mahatma Gandhi | Jawaharlal Nehru | Dr. Rajendra Prasad | Sardar Patel | Dr. Rajendra Prasad | Dr. Rajendra Prasad was India's first President | medium | Indian History | 1 | single |

## Multiple Choice Questions

For multiple choice questions, set **Question Type** to `multiple` and use comma-separated values in **Correct Answer**:

| Question Text | Choice 1 | Choice 2 | Choice 3 | Choice 4 | Correct Answer | Question Type |
|---------------|----------|----------|----------|----------|----------------|---------------|
| Which numbers are prime? | 2 | 4 | 7 | 9 | 1,3 | multiple |

Or use the exact text of multiple choices:

| Question Text | Choice 1 | Choice 2 | Choice 3 | Choice 4 | Correct Answer | Question Type |
|---------------|----------|----------|----------|----------|----------------|---------------|
| Which numbers are prime? | 2 | 4 | 7 | 9 | 2,7 | multiple |

## True/False Questions

For True/False questions:

| Question Text | Choice 1 | Choice 2 | Correct Answer | Question Type |
|---------------|----------|----------|----------------|---------------|
| The Earth is round | True | False | True | true_false |

## Important Notes

1. **First row should contain column headers**
2. **Data starts from the second row** (or the row specified with `--start-row`)
3. **Empty rows are automatically skipped**
4. **Correct Answer** can be:
   - The exact text of the correct choice (case-insensitive)
   - The choice number (1, 2, 3, 4, etc.)
   - For multiple choice: comma-separated numbers or choice texts (e.g., "1,3" or "Water,Oxygen")
5. **Question Type** values:
   - `single` - Single choice (radio buttons)
   - `multiple` - Multiple choice (checkboxes)
   - `true_false` - True/False question
6. **Difficulty** values:
   - `easy`
   - `medium`
   - `hard`

## File Processing

After running the import command:

- **Successful imports**: Files are moved to the `processed/` folder
- **Failed imports**: Files are moved to the `failed/` folder with an error log file (`filename_errors.txt`)

## Error Handling

If there are errors during import:

1. Check the error log file in the `failed/` folder
2. Fix the issues in your Excel file
3. Move the file back to the `inbox/` folder
4. Run the import command again

## Tips

1. **Save as .xlsx format** for best compatibility
2. **Use clear column headers** in the first row
3. **Check for empty rows** before importing
4. **Verify correct answers** match one of the choices exactly (case-insensitive)
5. **Use consistent formatting** for Question Type and Difficulty values

## Comparison with CSV Import

| Feature | Excel Import | CSV Import |
|---------|--------------|------------|
| File Format | .xlsx, .xls | .csv |
| Multiple Sheets | ✅ Yes (with `--sheet-name`) | ❌ No |
| Header Row | ✅ Configurable | ✅ Automatic |
| Column Formatting | ✅ Excel formatting preserved | ✅ Plain text |
| Bracket Support | ✅ Not needed (no comma issues) | ✅ Needed for commas |
| File Size | ⚠️ Limited by Excel limits | ✅ Can handle larger files |

