# CSV Import Format Sample

Based on the import script, here are sample CSV formats that will work:

## Format 1: With Brackets (Recommended for fields with commas)

```csv
[Category],[Exam],[Question Text],[Choice 1],[Choice 2],[Choice 3],[Choice 4],[Correct Answer],[Explanation],[Difficulty],[Topic],[Points],[Question Type]
[Math],[Basic Math Test],[What is 2 + 2?],[3],[4],[5],[6],[4],[Simple addition: 2 + 2 = 4],[easy],[Arithmetic],[1],[single]
[Science],[Science Quiz],[What is H2O?],[Water],[Oxygen],[Hydrogen],[Carbon Dioxide],[Water],[H2O is the chemical formula for water],[easy],[Chemistry],[1],[single]
[History],[World History],[Who was the first President of India?],[Mahatma Gandhi],[Jawaharlal Nehru],[Dr. Rajendra Prasad],[Sardar Patel],[Dr. Rajendra Prasad],[Dr. Rajendra Prasad was India's first President],[medium],[Indian History],[1],[single]
```

## Format 2: Without Brackets (Simpler format)

```csv
Category,Exam,Question Text,Choice 1,Choice 2,Choice 3,Choice 4,Correct Answer,Explanation,Difficulty,Topic,Points,Question Type
Math,Basic Math Test,What is 2 + 2?,3,4,5,6,4,Simple addition: 2 + 2 = 4,easy,Arithmetic,1,single
Science,Science Quiz,What is H2O?,Water,Oxygen,Hydrogen,Carbon Dioxide,Water,H2O is the chemical formula for water,easy,Chemistry,1,single
History,World History,Who was the first President of India?,Mahatma Gandhi,Jawaharlal Nehru,Dr. Rajendra Prasad,Sardar Patel,Dr. Rajendra Prasad,Dr. Rajendra Prasad was India's first President,medium,Indian History,1,single
```

## Column Names Accepted

### Required Columns:
- **Exam** (or `exam`, `Exam`) - Name of the exam
- **Question Text** (or `question_text`, `Question Text`, `question`, `Question`) - The question

### Choice Columns (at least one required):
- **Choice 1** (or `choice_1`, `Choice 1`, `option_1`, `Option 1`)
- **Choice 2** (or `choice_2`, `Choice 2`, `option_2`, `Option 2`)
- **Choice 3** (or `choice_3`, `Choice 3`, `option_3`, `Option 3`)
- **Choice 4** (or `choice_4`, `Choice 4`, `option_4`, `Option 4`)
- Up to Choice 6 supported

### Optional Columns:
- **Category** (or `category`, `Category`, `cat`) - Category name
- **Topic** (or `topic`, `Topic`) - Topic name
- **Correct Answer** (or `correct_answer`, `Correct Answer`, `answer`, `Answer`) - The correct choice text or number
- **Explanation** (or `explanation`, `Explanation`) - Explanation of the answer
- **Difficulty** (or `difficulty`, `Difficulty`, `diff`) - easy, medium, or hard
- **Points** (or `points`, `Points`, `point`) - Points for the question (default: 1)
- **Question Type** (or `question_type`, `Question Type`, `type`, `Type`) - single, multiple, or true_false (default: single)

## Important Notes:

1. **Brackets are optional** but recommended if your data contains commas
2. **Column names are case-insensitive** - `Exam`, `exam`, `EXAM` all work
3. **Spaces vs Underscores** - Both `Question Text` and `question_text` work
4. **Correct Answer** can be:
   - The exact text of the correct choice (case-insensitive)
   - The choice number (1, 2, 3, 4, etc.)
   - Multiple numbers separated by comma for multiple choice questions (e.g., "1,3")
5. **Question Type**: 
   - `single` - Single choice (radio buttons)
   - `multiple` - Multiple choice (checkboxes)
   - `true_false` - True/False question
6. **Difficulty**: `easy`, `medium`, or `hard` (default: `medium`)

## Example with Multiple Choice:

```csv
[Category],[Exam],[Question Text],[Choice 1],[Choice 2],[Choice 3],[Choice 4],[Correct Answer],[Explanation],[Difficulty],[Topic],[Points],[Question Type]
[Math],[Advanced Test],[Which numbers are prime?],[2],[4],[7],[9],[1,3],[2 and 7 are prime numbers, 4 and 9 are composite],[medium],[Number Theory],[2],[multiple]
```

## Example with True/False:

```csv
[Category],[Exam],[Question Text],[Choice 1],[Choice 2],[Correct Answer],[Explanation],[Difficulty],[Topic],[Points],[Question Type]
[Science],[Science Quiz],[The Earth is round],[True],[False],[True],[The Earth is approximately spherical in shape],[easy],[Geography],[1],[true_false]
```

## Common Issues to Avoid:

1. ❌ Missing required columns (`Exam` and `Question Text`)
2. ❌ No choice columns provided
3. ❌ Correct Answer doesn't match any choice (check spelling/case)
4. ❌ Invalid Question Type (must be: single, multiple, or true_false)
5. ❌ Invalid Difficulty (must be: easy, medium, or hard)

