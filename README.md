import React, { useState } from 'react';

const IELTSSpeakingAcademic = () => {
  const [studentName, setStudentName] = useState('');
  const [activeTab, setActiveTab] = useState('rating-scales');
  const [fontSize, setFontSize] = useState(14.5);
  const [scores, setScores] = useState({
    fluency: 6,
    lexical: 6,
    grammar: 6,
    pronunciation: 6
  });

  const descriptors = {
    fluency: {
      name: 'Fluency & Coherence',
      9: 'Fluent with only very occasional repetition or self-correction. Any hesitation is used only to prepare content of next utterance. Speech is situationally appropriate and cohesive. Topic development is fully coherent.',
      8: 'Speaks fluently with only occasional repetition or self-correction. Hesitation is usually content-related and does not obscure meaning. Speech is appropriately organized.',
      7: 'Generally speaks fluently with occasional repetition or hesitation. Some coherence issues may occur but meaning is clear.',
      6: 'Able to keep going but coherence may be lost due to hesitation, repetition and/or self-correction.',
      5: 'Usually produces continuous speech but may be hesitant. Some coherence and organization issues.',
      4: 'Produces long turns but frequently hesitates and repeats.',
      3: 'Speaks in short, often disconnected utterances.',
      2: 'Long pauses between utterances; difficulty sustaining speech.',
      1: 'Very little communication; frequent long pauses.'
    },
    lexical: {
      name: 'Lexical Resource',
      9: 'Uses vocabulary with total flexibility and precision. Uses idiomatic language naturally and accurately.',
      8: 'Uses a wide range of vocabulary with precision. Uses less common words accurately.',
      7: 'Uses sufficient vocabulary to discuss topics at length. Paraphrases successfully.',
      6: 'Has a resource sufficient to discuss topics at length. Vocabulary is appropriate but may be repetitive.',
      5: 'Manages most topics though with some repetition.',
      4: 'Limited vocabulary range. Difficulty with less familiar topics.',
      3: 'Vocabulary is mainly simple with frequent repetition.',
      2: 'Limited vocabulary restricted to familiar topics.',
      1: 'Minimal vocabulary; communication severely limited.'
    },
    grammar: {
      name: 'Grammatical Range & Accuracy',
      9: 'Uses a wide range of structures flexibly and accurately. Rare errors only.',
      8: 'Uses a wide range of structures with flexibility. Occasional errors but do not impede communication.',
      7: 'Uses a variety of structures with some flexibility. Some errors in complex forms.',
      6: 'Produces a mix of short and complex structures. Errors frequently occur in complex structures.',
      5: 'Makes some use of complex structures but with frequent errors.',
      4: 'Relies on basic structures; some attempt at complex forms.',
      3: 'Uses mainly simple structures with frequent errors.',
      2: 'Limited range; basic structures with many errors.',
      1: 'Minimal grammatical control; serious errors throughout.'
    },
    pronunciation: {
      name: 'Pronunciation',
      9: 'Effortless to understand. Native-like pronunciation with appropriate stress, rhythm and intonation.',
      8: 'Pronunciation is clear and natural. Minimal effort for listener.',
      7: 'Generally clear and easy to understand. Occasional errors but do not obscure meaning.',
      6: 'Pronunciation is generally clear; accent is noticeable but does not impede communication.',
      5: 'Understandable despite errors in pronunciation and intonation.',
      4: 'Accent is noticeable and may cause some difficulty understanding.',
      3: 'Accent and errors frequently make comprehension difficult.',
      2: 'Accent and pronunciation errors significantly impede communication.',
      1: 'Very difficult to understand due to pronunciation issues.'
    }
  };

  const criteria = ['fluency', 'lexical', 'grammar', 'pronunciation'];

  const updateScore = (criterion, value) => {
    const numValue = parseFloat(value);
    if (!isNaN(numValue) && numValue >= 1 && numValue <= 9) {
      setScores({ ...scores, [criterion]: numValue });
    }
  };

  const calculateOverallBand = () => {
    const average = (scores.fluency + scores.lexical + scores.grammar + scores.pronunciation) / 4;
    return Math.round(average * 2) / 2;
  };

  const getBandLevel = (band) => {
    if (band >= 8.5) return { level: 'Expert User', desc: 'Fluent, native-like proficiency' };
    if (band >= 8.0) return { level: 'Very Good', desc: 'Fully functional English' };
    if (band >= 7.0) return { level: 'Good', desc: 'Operational effective language use' };
    if (band >= 6.0) return { level: 'Competent', desc: 'Generally effective language use' };
    if (band >= 5.0) return { level: 'Modest', desc: 'Modest language use' };
    if (band >= 4.0) return { level: 'Limited', desc: 'Limited language use' };
    return { level: 'Extremely Limited', desc: 'Severe problems with communication' };
  };

  const generateComment = () => {
    const band = calculateOverallBand();
    const name = studentName || 'Student';
    let comment = '';

    if (band >= 8.5) {
      comment = `${name} demonstrates expert-level English proficiency with native-like fluency, extensive vocabulary, accurate grammar, and clear pronunciation. Performance indicates readiness for advanced academic and professional environments.`;
    } else if (band >= 8.0) {
      comment = `${name} shows very good English proficiency with fluent speech, wide vocabulary range, accurate structures, and clear pronunciation. Minor errors do not impede communication.`;
    } else if (band >= 7.0) {
      comment = `${name} demonstrates good English proficiency with generally fluent speech and adequate vocabulary. Some errors in complex structures occur but do not significantly affect comprehension.`;
    } else if (band >= 6.0) {
      comment = `${name} shows competent English proficiency. Communication is generally effective, though hesitations and occasional errors occur.`;
    } else if (band >= 5.0) {
      comment = `${name} demonstrates modest English proficiency. While basic communication is possible, frequent hesitations and vocabulary limitations are evident.`;
    } else {
      comment = `${name} shows limited English proficiency. Significant challenges impede communication. Intensive English language instruction is recommended.`;
    }

    return comment;
  };

  const clearAll = () => {
    if (window.confirm('Clear all scores?')) {
      setStudentName('');
      setScores({
        fluency: 6,
        lexical: 6,
        grammar: 6,
        pronunciation: 6
      });
    }
  };

  const band = calculateOverallBand();
  const { level, desc } = getBandLevel(band);

  return (
    <div style={{ fontSize: fontSize + 'px' }}>
      <style>{`
        * {
          margin: 0;
          padding: 0;
          box-sizing: border-box;
        }

        html {
          scroll-behavior: smooth;
        }

        body {
          font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
          line-height: 1.6;
          color: #333;
          background: #fafafa;
        }

        .container {
          max-width: 1200px;
          margin: 0 auto;
          padding: 0 20px;
        }

        header {
          background: white;
          border-bottom: 1px solid #ddd;
          padding: 30px 0;
          margin-bottom: 40px;
        }

        header h1 {
          font-size: 1.75em;
          font-weight: 600;
          letter-spacing: -0.5px;
          margin-bottom: 5px;
          color: #222;
        }

        header p {
          font-size: 0.875em;
          color: #666;
          font-weight: 400;
        }

        .controls {
          background: white;
          padding: 20px;
          border-radius: 4px;
          margin-bottom: 30px;
          display: flex;
          gap: 30px;
          align-items: center;
          border: 1px solid #eee;
          flex-wrap: wrap;
        }

        .control-group {
          display: flex;
          align-items: center;
          gap: 10px;
        }

        .control-group label {
          font-size: 0.8125em;
          font-weight: 600;
          color: #666;
          text-transform: uppercase;
          letter-spacing: 0.5px;
        }

        .control-group select,
        .control-group input {
          padding: 6px 10px;
          border: 1px solid #ddd;
          border-radius: 3px;
          font-size: 0.875em;
          background: white;
          cursor: pointer;
        }

        .control-group input[type="text"] {
          padding: 8px 12px;
          min-width: 200px;
        }

        .control-group button {
          padding: 6px 15px;
          background: #f0f0f0;
          border: 1px solid #ddd;
          border-radius: 3px;
          cursor: pointer;
          font-size: 0.8125em;
          font-weight: 600;
          color: #333;
          transition: all 0.2s;
        }

        .control-group button:hover {
          background: #e8e8e8;
          border-color: #999;
        }

        .tabs {
          display: flex;
          gap: 0;
          border-bottom: 2px solid #ddd;
          margin-bottom: 30px;
          background: white;
        }

        .tab-button {
          padding: 15px 25px;
          border: none;
          background: none;
          cursor: pointer;
          font-size: 0.875em;
          font-weight: 600;
          color: #999;
          position: relative;
          transition: all 0.2s;
          border-bottom: 2px solid transparent;
          margin-bottom: -2px;
        }

        .tab-button:hover {
          color: #333;
        }

        .tab-button.active {
          color: #333;
          border-bottom-color: #333;
        }

        .tab-content {
          display: none;
        }

        .tab-content.active {
          display: block;
        }

        .input-section {
          background: white;
          padding: 30px;
          border-radius: 4px;
          border: 1px solid #eee;
          margin-bottom: 30px;
        }

        .input-section h2 {
          font-size: 1em;
          font-weight: 600;
          color: #333;
          margin-bottom: 20px;
          text-transform: uppercase;
          letter-spacing: 0.5px;
          border-bottom: 1px solid #eee;
          padding-bottom: 15px;
        }

        .input-section h3 {
          font-size: 0.875em;
          font-weight: 600;
          color: #666;
          margin: 25px 0 15px 0;
          text-transform: uppercase;
          letter-spacing: 0.5px;
        }

        .input-section p {
          font-size: 0.8125em;
          color: #666;
          margin-bottom: 20px;
          line-height: 1.6;
        }

        .criterion-inputs {
          display: grid;
          grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
          gap: 20px;
          margin-bottom: 25px;
        }

        .input-field {
          display: flex;
          flex-direction: column;
        }

        .input-field label {
          font-size: 0.8125em;
          font-weight: 600;
          color: #333;
          margin-bottom: 8px;
        }

        .input-field input {
          padding: 10px 12px;
          border: 1px solid #ddd;
          border-radius: 3px;
          font-size: 0.875em;
          background: white;
        }

        .input-field input:focus {
          outline: none;
          border-color: #333;
          box-shadow: 0 0 0 2px rgba(51, 51, 51, 0.1);
        }

        .score-display {
          background: white;
          padding: 30px;
          border-radius: 4px;
          border: 1px solid #eee;
          margin-bottom: 30px;
        }

        .score-display h2 {
          font-size: 1em;
          font-weight: 600;
          color: #333;
          margin-bottom: 20px;
          text-transform: uppercase;
          letter-spacing: 0.5px;
          border-bottom: 1px solid #eee;
          padding-bottom: 15px;
        }

        .score-result {
          display: grid;
          grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
          gap: 20px;
          margin-bottom: 20px;
        }

        .result-box {
          display: flex;
          flex-direction: column;
          align-items: center;
          padding: 20px;
          background: #fafafa;
          border: 1px solid #eee;
          border-radius: 4px;
        }

        .result-label {
          font-size: 0.75em;
          color: #999;
          text-transform: uppercase;
          letter-spacing: 0.5px;
          margin-bottom: 10px;
          font-weight: 600;
        }

        .result-value {
          font-size: 2.25em;
          font-weight: 700;
          color: #333;
          font-family: 'Courier New', monospace;
        }

        .result-level {
          font-size: 0.8125em;
          color: #666;
          margin-top: 8px;
          font-weight: 500;
          text-align: center;
        }

        .descriptor-section {
          background: #fafafa;
          padding: 20px;
          border-radius: 4px;
          margin-top: 20px;
          border-left: 3px solid #333;
        }

        .descriptor-label {
          font-size: 0.75em;
          font-weight: 600;
          color: #666;
          text-transform: uppercase;
          letter-spacing: 0.5px;
          margin-bottom: 10px;
        }

        .descriptor-text {
          font-size: 0.8125em;
          line-height: 1.7;
          color: #666;
        }

        .table-section {
          background: white;
          padding: 30px;
          border-radius: 4px;
          border: 1px solid #eee;
          margin-bottom: 30px;
          overflow-x: auto;
        }

        .table-section h2 {
          font-size: 1em;
          font-weight: 600;
          color: #333;
          margin-bottom: 20px;
          text-transform: uppercase;
          letter-spacing: 0.5px;
          border-bottom: 1px solid #eee;
          padding-bottom: 15px;
        }

        table {
          width: 100%;
          border-collapse: collapse;
          font-size: 0.8125em;
        }

        th {
          background: #fafafa;
          padding: 12px;
          text-align: left;
          font-weight: 600;
          color: #333;
          border-bottom: 2px solid #ddd;
          font-size: 0.75em;
          text-transform: uppercase;
          letter-spacing: 0.5px;
        }

        td {
          padding: 12px;
          border-bottom: 1px solid #eee;
          color: #666;
        }

        tr:hover {
          background: #fafafa;
        }

        footer {
          background: white;
          padding: 30px 0;
          margin-top: 50px;
          border-top: 1px solid #eee;
          text-align: center;
          font-size: 0.75em;
          color: #999;
        }

        @media (max-width: 768px) {
          header h1 {
            font-size: 1.375em;
          }

          .controls {
            flex-direction: column;
            gap: 15px;
            align-items: flex-start;
          }

          .control-group {
            width: 100%;
            flex-direction: column;
            align-items: flex-start;
          }

          .control-group input[type="text"] {
            width: 100%;
          }

          .criterion-inputs {
            grid-template-columns: 1fr;
          }

          .score-result {
            grid-template-columns: 1fr;
          }

          .tab-button {
            padding: 12px 15px;
            font-size: 0.8125em;
          }

          .input-section,
          .score-display,
          .table-section {
            padding: 20px;
          }

          th, td {
            padding: 8px;
          }
        }
      `}</style>

      <header>
        <div className="container">
          <h1>IELTS Speaking Band Descriptors</h1>
          <p>Interactive rating scales · Mark & level calculator</p>
        </div>
      </header>

      <div className="container">
        {/* Controls */}
        <div className="controls">
          <div className="control-group">
            <label>Student Name:</label>
            <input
              type="text"
              placeholder="Enter name"
              value={studentName}
              onChange={(e) => setStudentName(e.target.value)}
            />
          </div>
          <div className="control-group">
            <label>Font Size:</label>
            <select value={fontSize} onChange={(e) => setFontSize(parseFloat(e.target.value))}>
              <option value="14">14px</option>
              <option value="14.5">14.5px</option>
              <option value="15">15px</option>
              <option value="16">16px</option>
              <option value="18">18px</option>
            </select>
          </div>
          <div className="control-group">
            <button onClick={clearAll}>Clear All</button>
          </div>
        </div>

        {/* Tabs */}
        <div className="tabs">
          <button
            className={`tab-button ${activeTab === 'rating-scales' ? 'active' : ''}`}
            onClick={() => setActiveTab('rating-scales')}
          >
            Rating Scales
          </button>
          <button
            className={`tab-button ${activeTab === 'mark-level' ? 'active' : ''}`}
            onClick={() => setActiveTab('mark-level')}
          >
            Mark & Level
          </button>
        </div>

        {/* Tab 1: Rating Scales */}
        {activeTab === 'rating-scales' && (
          <>
            <div className="input-section">
              <h2>Assess by Criterion</h2>
              <p>Enter each criterion score from 1–9 in 0.5 steps. Scores are displayed below with official IELTS band descriptors.</p>

              <h3>Score Each Criterion</h3>
              <div className="criterion-inputs">
                {criteria.map(criterion => (
                  <div key={criterion} className="input-field">
                    <label>{descriptors[criterion].name}</label>
                    <input
                      type="number"
                      min="1"
                      max="9"
                      step="0.5"
                      value={scores[criterion]}
                      onChange={(e) => updateScore(criterion, e.target.value)}
                    />
                  </div>
                ))}
              </div>
            </div>

            <div className="score-display">
              <h2>Criterion Descriptors</h2>
              {criteria.map(criterion => (
                <div key={criterion} className="descriptor-section">
                  <div className="descriptor-label">
                    {descriptors[criterion].name}: {scores[criterion]}/9
                  </div>
                  <div className="descriptor-text">
                    {descriptors[criterion][Math.round(scores[criterion])]}
                  </div>
                </div>
              ))}
            </div>
          </>
        )}

        {/* Tab 2: Mark & Level */}
        {activeTab === 'mark-level' && (
          <>
            <div className="input-section">
              <h2>Overall Mark & Level</h2>
              <p>Enter each criterion score from 1–9 in 0.5 steps. The four-criterion average is rounded to the nearest 0.5.</p>

              <h3>Criterion Scores</h3>
              <div className="criterion-inputs">
                {criteria.map(criterion => (
                  <div key={criterion} className="input-field">
                    <label>{descriptors[criterion].name}</label>
                    <input
                      type="number"
                      min="1"
                      max="9"
                      step="0.5"
                      value={scores[criterion]}
                      onChange={(e) => updateScore(criterion, e.target.value)}
                    />
                  </div>
                ))}
              </div>
            </div>

            <div className="score-display">
              <h2>Overall Band & Level</h2>
              <div className="score-result">
                <div className="result-box">
                  <div className="result-label">Overall Band</div>
                  <div className="result-value">{band}</div>
                  <div className="result-level">
                    <strong>{level}</strong>
                    <br />
                    <small>{desc}</small>
                  </div>
                </div>
              </div>

              <div className="descriptor-section">
                <div className="descriptor-label">Assessment Comment</div>
                <div className="descriptor-text">
                  <strong>{studentName || 'Student'}</strong> – {generateComment()}
                </div>
              </div>
            </div>

            <div className="table-section">
              <h2>IELTS Band Scale Conversion</h2>
              <table>
                <thead>
                  <tr>
                    <th>Average of 4 Criteria</th>
                    <th>IELTS Band</th>
                    <th>Level</th>
                    <th>Description</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>8.5 – 9.0</td>
                    <td><strong>9</strong></td>
                    <td>Expert User</td>
                    <td>Fluent, native-like proficiency</td>
                  </tr>
                  <tr>
                    <td>8.0 – 8.5</td>
                    <td><strong>8</strong></td>
                    <td>Very Good</td>
                    <td>Fully functional English</td>
                  </tr>
                  <tr>
                    <td>7.0 – 7.5</td>
                    <td><strong>7</strong></td>
                    <td>Good</td>
                    <td>Operational effective language use</td>
                  </tr>
                  <tr>
                    <td>6.0 – 6.5</td>
                    <td><strong>6</strong></td>
                    <td>Competent</td>
                    <td>Generally effective language use</td>
                  </tr>
                  <tr>
                    <td>5.0 – 5.5</td>
                    <td><strong>5</strong></td>
                    <td>Modest</td>
                    <td>Modest language use</td>
                  </tr>
                  <tr>
                    <td>4.0 – 4.5</td>
                    <td><strong>4</strong></td>
                    <td>Limited</td>
                    <td>Limited language use</td>
                  </tr>
                  <tr>
                    <td>Below 4.0</td>
                    <td><strong>1-3</strong></td>
                    <td>Extremely Limited</td>
                    <td>Severe problems with communication</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </>
        )}
      </div>

      <footer>
        <div className="container">
          <p>Based on official IELTS Speaking Band Descriptors · British Council, IDP, Cambridge Assessment English</p>
        </div>
      </footer>
    </div>
  );
};

export default IELTSSpeakingAcademic;
