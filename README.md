# ResumeAI Predictor 🚀

An AI-powered resume analyzer that predicts your acceptance chances at top tech companies using Claude AI.

![ResumeAI Predictor](https://img.shields.io/badge/React-18+-61DAFB?style=flat&logo=react)
![Claude AI](https://img.shields.io/badge/Claude-Sonnet%204-8E75B2?style=flat)
![License](https://img.shields.io/badge/license-MIT-green)

## ✨ Features

- **AI-Powered Analysis**: Uses Claude Sonnet 4 to analyze resumes with expert-level insights
- **Company Predictions**: Get acceptance probability scores for 20+ top tech companies
- **Detailed Feedback**: Receive specific strengths, weaknesses, and improvement suggestions
- **Custom Companies**: Add any company you want to evaluate beyond the default list
- **Beautiful UI**: Modern, dark-themed interface with smooth animations
- **File Upload**: Support for both text paste and .txt file upload
- **Missing Skills Detection**: Identifies specific skills needed for each company

## 🎯 What You Get

1. **Overall Resume Score** (0-100)
2. **Company-Specific Predictions** with reasoning
3. **Your Top Strengths** highlighted
4. **Priority Improvements** to make
5. **Missing Skills** per company
6. **Detailed Issues** categorized as errors, warnings, or tips
7. **Actionable Suggestions** for every issue found

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ installed
- Anthropic API access (available at https://api.anthropic.com)
- React development environment

### Installation

1. **Clone or download the project**
```bash
# If using git
git clone <your-repo-url>
cd resume-predictor

# Or just download the .jsx file
```

2. **Install dependencies**
```bash
npm install react react-dom
```

3. **Add the component to your React app**
```jsx
import ResumePredictor from './resume-predictor-refactored.jsx';

function App() {
  return <ResumePredictor />;
}
```

4. **Run your app**
```bash
npm start
```

## 🔧 Configuration

### API Setup

The app uses the Anthropic Claude API. You have two options:

#### Option 1: Direct API Calls (Current Implementation)
The app currently makes direct calls to the Anthropic API. **Note**: This exposes your API in the browser, which is not secure for production.

For production, you should:
1. Create a backend endpoint
2. Store API keys securely on the server
3. Proxy requests through your backend

#### Option 2: Backend Proxy (Recommended for Production)
```javascript
// In your backend (e.g., Express.js)
app.post('/api/analyze-resume', async (req, res) => {
  const { resumeText, companies } = req.body;
  
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': process.env.ANTHROPIC_API_KEY,
      'anthropic-version': '2023-06-01'
    },
    body: JSON.stringify({
      model: 'claude-sonnet-4-20250514',
      max_tokens: 1000,
      messages: [{ role: 'user', content: prompt }]
    })
  });
  
  const data = await response.json();
  res.json(data);
});
```

Then update the `analyzeResume` function in the component:
```javascript
const analyzeResume = async (resumeText, companies) => {
  const response = await fetch('/api/analyze-resume', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ resumeText, companies })
  });
  
  const data = await response.json();
  // ... rest of the code
};
```

### Customizing Companies

To modify the default company list, edit the `COMPANIES` constant:

```javascript
const COMPANIES = [
  "Your Company 1",
  "Your Company 2",
  // ... add more
];
```

### Customizing Colors

Adjust the color scheme in the `COLORS` constant:

```javascript
const COLORS = {
  high: "#00e5a0",    // Green for high scores
  medium: "#f5c842",  // Yellow for medium scores
  low: "#ff5c7c",     // Red for low scores
  // ...
};
```

## 📁 Project Structure

```
resume-predictor-refactored.jsx
├── Constants
│   ├── COMPANIES - List of tech companies
│   ├── API_CONFIG - API configuration
│   ├── COLORS - Color scheme
│   └── STEP - App states
├── Utilities
│   ├── parseJSON - Parse AI responses
│   ├── getScoreColor - Score-to-color mapping
│   └── createPrompt - Build AI prompt
├── API Service
│   └── analyzeResume - Call Claude API
├── Custom Hooks
│   ├── useCompanySelection - Manage company selection
│   └── useFileUpload - Handle file uploads
└── Components
    ├── ScoreBar - Animated progress bar
    ├── Tag - Category badges
    ├── Header - App header
    ├── CompanySelector - Company selection UI
    ├── ResumeInput - Text input/file upload
    ├── OverallScoreBanner - Score display
    ├── StrengthsAndImprovements - Feedback sections
    ├── CompanyPredictions - Company results
    ├── IssuesAndSuggestions - Detailed feedback
    └── App - Main component
```

## 🎨 Refactoring Improvements

The refactored version includes:

### 1. **Better Code Organization**
- Separated constants, utilities, and components
- Clear section boundaries with comments
- Logical grouping of related code

### 2. **Custom Hooks**
- `useCompanySelection` - Encapsulates company management logic
- `useFileUpload` - Handles file upload complexity

### 3. **Component Extraction**
- Broke down monolithic component into smaller, reusable pieces
- Each component has a single responsibility
- Easier to test and maintain

### 4. **Improved Maintainability**
- Constants at the top for easy configuration
- Separated API logic from UI logic
- Type-safe color management

### 5. **Better Error Handling**
- Dedicated validation function
- Clear error messages
- Proper loading states

### 6. **Enhanced Readability**
- Descriptive function names
- Consistent formatting
- Removed magic numbers/strings

## 💡 Usage Tips

### For Best Results

1. **Resume Length**: Keep your resume to 1-2 pages
2. **Format**: Plain text works best, avoid heavy formatting
3. **Content**: Include specific achievements, metrics, and technologies
4. **Keywords**: Use industry-standard terms and technologies
5. **Company Selection**: Start with 3-5 companies for faster analysis

### Common Issues

**Problem**: "Could not parse AI response"
- **Solution**: Check your API key and rate limits

**Problem**: Resume text truncated
- **Solution**: The app limits input to 6000 characters (configurable in `API_CONFIG`)

**Problem**: No companies selected
- **Solution**: Click on at least one company before analyzing

## 🔐 Security Considerations

⚠️ **Important**: Never expose API keys in production front-end code!

For production deployment:
1. Set up a backend server
2. Store API keys in environment variables
3. Use server-side API calls only
4. Implement rate limiting
5. Add user authentication if needed

## 📊 API Response Format

The Claude API returns a JSON structure:

```json
{
  "overallScore": 75,
  "summary": "Strong technical background with room for improvement...",
  "companies": [
    {
      "name": "Google",
      "acceptanceChance": 65,
      "reasoning": "Good algorithmic background but lacks...",
      "missingSkills": ["System Design", "Leadership"]
    }
  ],
  "errors": [
    {
      "type": "warning",
      "title": "Inconsistent Formatting",
      "description": "Your experience section uses mixed date formats",
      "suggestion": "Use MM/YYYY format consistently"
    }
  ],
  "strengths": [
    "Strong problem-solving skills",
    "Diverse tech stack experience"
  ],
  "topImprovements": [
    "Add quantifiable metrics",
    "Include leadership examples"
  ]
}
```

## 🛠️ Troubleshooting

### Build Issues
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm install
```

### API Errors
```bash
# Check API key format
# Verify rate limits
# Ensure proper request format
```

### Style Issues
```bash
# Verify Google Fonts are loading
# Check for CSS conflicts
# Clear browser cache
```

## 📝 License

MIT License - feel free to use this in your projects!

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📧 Support

For issues or questions:
- Check the troubleshooting section
- Review the API documentation at https://docs.anthropic.com
- Open an issue in the repository

## 🎯 Roadmap

- [ ] Add PDF resume parsing
- [ ] Support for multiple resume formats
- [ ] Resume comparison feature
- [ ] Export analysis as PDF report
- [ ] Save and track improvements over time
- [ ] A/B testing different resume versions
- [ ] Industry-specific analysis modes

## ⭐ Acknowledgments

- Powered by [Anthropic Claude AI](https://www.anthropic.com)
- UI inspired by modern SaaS dashboards
- Fonts: DM Sans & Space Grotesk from Google Fonts

---

**Made with ⚡ by developers, for developers**
