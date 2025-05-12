# Survey Form Builder (ReactJS)
## Date:12-05-2025

## AIM
To create a Survey Form Builder using ReactJS..

## ALGORITHM
### STEP 1 Initialize States
mode ← 'build' (default mode)

questions ← [] (holds all survey questions)

currentQuestion ← { text: '', type: 'text', options: '' } (holds input while building)

editingIndex ← null (tracks which question is being edited)

responses ← {} (user responses to survey)

submitted ← false (indicates whether survey was submitted)

### STEP 2 Switch Modes
Toggle mode between 'build' and 'fill' when the toggle button is clicked.

Reset submitted to false when entering Fill Mode.

### STEP 3 Build Mode Logic
#### a. Add/Update Question
If currentQuestion.text is not empty:

If type is "radio" or "checkbox", split currentQuestion.options by commas → optionsArray.

Construct a questionObject:

If editingIndex is not null, replace question at that index.

Else, push questionObject into questions.

Reset currentQuestion and editingIndex.

#### b. Edit Question
Set currentQuestion to the selected question’s data.

Set editingIndex to the question’s index.

#### c. Delete Question
Remove the selected question from questions.

### STEP 4 Fill Mode Logic
#### a. Render Form
For each question in questions:

If type is "text", render a text input.

If type is "radio", render radio buttons for each option.

If type is "checkbox", render checkboxes.

#### b. Capture Responses
On input change:

For "text" and "radio", update responses[index] = value.

For "checkbox":

If option exists in responses[index], remove it.

Else, add it to the array.

### STEP 5 Submit Form
When user clicks "Submit":

Set submitted = true.

Display a summary using the questions and responses.

### STEP 6 Display Summary
Iterate over questions.

Display the response from responses[i] for each question:

If it's an array, join it with commas.

If empty, show "No response".


## PROGRAM

DEVELOPED BY : NARRA RAMYA

REG NO:212223040128
```
import React, { useState } from 'react';

export default function SurveyFormBuilder() {
  const [mode, setMode] = useState('build');
  const [questions, setQuestions] = useState([]);
  const [questionText, setQuestionText] = useState('');
  const [questionType, setQuestionType] = useState('text');
  const [options, setOptions] = useState('');
  const [responses, setResponses] = useState({});

  const addQuestion = () => {
    if (!questionText.trim()) return;

    const newQuestion = {
      id: Date.now(),
      text: questionText,
      type: questionType,
      options: questionType !== 'text' ? options.split(',').map(opt => opt.trim()) : [],
    };
    setQuestions([...questions, newQuestion]);
    setQuestionText('');
    setQuestionType('text');
    setOptions('');
  };

  const removeQuestion = (id) => {
    setQuestions(questions.filter(q => q.id !== id));
  };

  const handleResponseChange = (id, value) => {
    setResponses({ ...responses, [id]: value });
  };

  const handleCheckboxChange = (id, option) => {
    const current = responses[id] || [];
    if (current.includes(option)) {
      setResponses({ ...responses, [id]: current.filter(o => o !== option) });
    } else {
      setResponses({ ...responses, [id]: [...current, option] });
    }
  };

  return (
    <div
      style={{
        padding: '20px',
        maxWidth: '800px',
        margin: 'auto',
        backgroundImage: 'url("https://www.transparenttextures.com/patterns/wood-pattern.png")', // You can use any URL or local image
        backgroundSize: 'cover',
        backgroundRepeat: 'no-repeat',
        minHeight: '100vh',
        color: 'black',
        fontFamily: 'Arial, sans-serif'
      }}
    >
      <h1>Survey Builder</h1>
      <button onClick={() => setMode(mode === 'build' ? 'fill' : 'build')}>
        Switch to {mode === 'build' ? 'Fill' : 'Build'} Mode
      </button>

      {mode === 'build' ? (
        <div>
          <h2>Build Mode</h2>
          <input
            placeholder="Enter question text"
            value={questionText}
            onChange={(e) => setQuestionText(e.target.value)}
          />
          <select value={questionType} onChange={(e) => setQuestionType(e.target.value)}>
            <option value="text">Text</option>
            <option value="radio">Radio</option>
            <option value="checkbox">Checkbox</option>
          </select>
          {(questionType === 'radio' || questionType === 'checkbox') && (
            <input
              placeholder="Enter options, comma separated"
              value={options}
              onChange={(e) => setOptions(e.target.value)}
            />
          )}
          <button onClick={addQuestion}>Add Question</button>

          <ul>
            {questions.map(q => (
              <li key={q.id}>
                {q.text} ({q.type})
                <button onClick={() => removeQuestion(q.id)}>Remove</button>
              </li>
            ))}
          </ul>
        </div>
      ) : (
        <div>
          <h2>Fill Mode</h2>
          <form onSubmit={(e) => { e.preventDefault(); alert(JSON.stringify(responses, null, 2)); }}>
            {questions.map(q => (
              <div key={q.id} style={{ marginBottom: '10px' }}>
                <label>{q.text}</label>
                {q.type === 'text' && (
                  <input
                    type="text"
                    value={responses[q.id] || ''}
                    onChange={(e) => handleResponseChange(q.id, e.target.value)}
                  />
                )}
                {q.type === 'radio' && q.options.map(opt => (
                  <label key={opt} style={{ display: 'block' }}>
                    <input
                      type="radio"
                      name={`question-${q.id}`}
                      value={opt}
                      checked={responses[q.id] === opt}
                      onChange={() => handleResponseChange(q.id, opt)}
                    />
                    {opt}
                  </label>
                ))}
                {q.type === 'checkbox' && q.options.map(opt => (
                  <label key={opt} style={{ display: 'block' }}>
                    <input
                      type="checkbox"
                      checked={(responses[q.id] || []).includes(opt)}
                      onChange={() => handleCheckboxChange(q.id, opt)}
                    />
                    {opt}
                  </label>
                ))}
              </div>
            ))}
            <button type="submit">Submit</button>
          </form>
        </div>
      )}
    </div>
  );
}

```


## OUTPUT
![WhatsApp Image 2025-05-12 at 11 00 02_f2dff122](https://github.com/user-attachments/assets/2ee54d09-ddb9-4aeb-b2f7-5458602460b0)

![WhatsApp Image 2025-05-12 at 10 59 03_d885fc9d](https://github.com/user-attachments/assets/e7cefc3c-a316-41fd-9bed-40add3212ee2)

![WhatsApp Image 2025-05-12 at 10 59 15_be183aa5](https://github.com/user-attachments/assets/de6f13db-2d15-41f7-950f-6fa9285c045f)

![WhatsApp Image 2025-05-12 at 10 59 28_ca1e912b](https://github.com/user-attachments/assets/be2af1ce-aaa4-40c6-b0b4-c6fd2c5b4598)

## RESULT
The program for creating Survey Form Builder using ReactJS is executed successfully.
