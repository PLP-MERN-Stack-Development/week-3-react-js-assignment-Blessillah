[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=19903075&assignment_repo_type=AssignmentRepo)
# React.js and Tailwind CSS Assignment

This assignment focuses on building a responsive React application using JSX and Tailwind CSS, implementing component architecture, state management, hooks, and API integration.

## Assignment Overview

You will:
1. Set up a React project with Vite and Tailwind CSS
2. Create reusable UI components
3. Implement state management using React hooks
4. Integrate with external APIs
5. Style your application using Tailwind CSS

## Getting Started

1. Accept the GitHub Classroom assignment invitation
2. Clone your personal repository that was created by GitHub Classroom
3. Install dependencies:
   ```
   npm install
   ```
4. Start the development server:
   ```
   npm run dev
   ```

## Files Included

- `Week3-Assignment.md`: Detailed assignment instructions
- Starter files for your React application:
  - Basic project structure
  - Pre-configured Tailwind CSS
  - Sample component templates

## Requirements

- Node.js (v18 or higher)
- npm or yarn
- Modern web browser
- Code editor (VS Code recommended)

## Project Structure

```
src/
├── components/       # Reusable UI components
├── pages/           # Page components
├── hooks/           # Custom React hooks
├── context/         # React context providers
├── api/             # API integration functions
├── utils/           # Utility functions
└── App.jsx          # Main application component
```

## Submission

Your work will be automatically submitted when you push to your GitHub Classroom repository. Make sure to:

1. Complete all required components and features
2. Implement proper state management with hooks
3. Integrate with at least one external API
4. Style your application with Tailwind CSS
5. Deploy your application and add the URL to your README.md

## Resources

- [React Documentation](https://react.dev/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [Vite Documentation](https://vitejs.dev/guide/)
- [React Router Documentation](https://reactrouter.com/)

import React, { useState, useEffect, useCallback } from 'react';

// Conceptual reusable UI Component: Header
// In a multi-file project, this would be in `src/components/Header.jsx`
const Header = ({ title }) => {
  return (
    <header className="bg-gradient-to-r from-emerald-500 to-teal-600 text-white p-6 shadow-lg rounded-b-xl">
      <div className="container mx-auto px-4 py-2">
        <h1 className="text-4xl md:text-5xl font-extrabold tracking-tight text-center">
          {title}
        </h1>
        <p className="mt-2 text-lg md:text-xl text-center opacity-90">
          Explore data fetched from a public API
        </p>
      </div>
    </header>
  );
};

// Conceptual reusable UI Component: LoadingSpinner
// In a multi-file project, this would be in `src/components/LoadingSpinner.jsx`
const LoadingSpinner = () => {
  return (
    <div className="flex justify-center items-center py-10">
      <div className="animate-spin rounded-full h-16 w-16 border-t-4 border-b-4 border-emerald-500"></div>
      <p className="ml-4 text-xl text-gray-600">Loading data...</p>
    </div>
  );
};

// Conceptual reusable UI Component: ErrorDisplay
// In a multi-file project, this would be in `src/components/ErrorDisplay.jsx`
const ErrorDisplay = ({ message }) => {
  return (
    <div className="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded-lg relative my-6 max-w-2xl mx-auto">
      <strong className="font-bold">Error!</strong>
      <span className="block sm:inline ml-2">{message}</span>
      <p className="text-sm mt-2">Please try refreshing the page or check your network connection.</p>
    </div>
  );
};

// Conceptual reusable UI Component: PostCard
// In a multi-file project, this would be in `src/components/PostCard.jsx`
const PostCard = ({ post }) => {
  return (
    <div className="bg-white rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 overflow-hidden border border-gray-200">
      <div className="p-6">
        <h3 className="text-xl font-semibold text-gray-900 mb-2 leading-tight capitalize">
          {post.title}
        </h3>
        <p className="text-sm text-gray-600 mb-4">
          User ID: <span className="font-medium text-emerald-600">{post.userId}</span>
        </p>
        <p className="text-gray-700 text-base line-clamp-3">
          {post.body}
        </p>
      </div>
      <div className="px-6 py-4 bg-gray-50 border-t border-gray-200">
        <button className="text-emerald-600 hover:text-emerald-800 font-medium text-sm">
          Read More &rarr;
        </button>
      </div>
    </div>
  );
};

// Main Application Component
// This would be `src/App.jsx` in your project structure
function App() {
  // State Management using React Hooks
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const [searchTerm, setSearchTerm] = useState('');

  // Function to fetch data from an external API
  // This would typically be in `src/api/posts.js`
  const fetchPosts = useCallback(async () => {
    setLoading(true);
    setError(null);
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/posts');
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      const data = await response.json();
      setPosts(data);
    } catch (err) {
      setError(`Failed to fetch posts: ${err.message}`);
      console.error("API fetch error:", err);
    } finally {
      setLoading(false);
    }
  }, []); // Empty dependency array means this function is created once

  // useEffect hook to call fetchPosts when the component mounts
  // This simulates data loading on application start
  useEffect(() => {
    fetchPosts();
  }, [fetchPosts]); // Dependency on fetchPosts to satisfy ESLint, though it's memoized by useCallback

  // Filtered posts based on search term
  const filteredPosts = posts.filter(post =>
    post.title.toLowerCase().includes(searchTerm.toLowerCase()) ||
    post.body.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    <div className="min-h-screen bg-gray-100 pb-10">
      <Header title="Public API Data Explorer" />

      <div className="container mx-auto px-4 mt-8">
        <div className="max-w-xl mx-auto mb-8">
          <input
            type="text"
            placeholder="Search posts by title or content..."
            className="w-full p-3 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-2 focus:ring-emerald-500 focus:border-transparent transition"
            value={searchTerm}
            onChange={(e) => setSearchTerm(e.target.value)}
          />
        </div>

        {loading && <LoadingSpinner />}

        {error && <ErrorDisplay message={error} />}

        {!loading && !error && (
          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
            {filteredPosts.length > 0 ? (
              filteredPosts.map(post => (
                <PostCard key={post.id} post={post} />
              ))
            ) : (
              <p className="col-span-full text-center text-gray-600 text-lg py-10">
                No posts found matching your search.
              </p>
            )}
          </div>
        )}
      </div>
    </div>
  );
}

export default App;
Week 3 Assignment: React.js and Tailwind CSS ApplicationThis repository contains the solution for the Week 3 assignment, focusing on building a responsive React application with Vite and Tailwind CSS. The application demonstrates core React concepts including component architecture, state management with hooks, and integration with an external API.Table of ContentsProject OverviewFeaturesGetting StartedPrerequisitesInstallationRunning the ApplicationProject StructureAPI IntegrationDeploymentSubmissionProject OverviewThis assignment involves creating a single-page React application that fetches and displays data from a public API. The application is styled entirely using Tailwind CSS, ensuring a clean, modern, and responsive user interface across various devices. It showcases the practical application of React hooks for state management and building reusable UI components.FeaturesData Display: Fetches and displays a list of posts from a public API.Search Functionality: Allows users to filter posts by title or body content.Loading State: Displays a loading spinner while data is being fetched.Error Handling: Provides a user-friendly message if the API call fails.Responsive Design: Adapts gracefully to different screen sizes (mobile, tablet, desktop) using Tailwind CSS.Reusable Components: Utilizes modular components for better code organization and reusability (e.g., Header, LoadingSpinner, ErrorDisplay, PostCard).Getting StartedFollow these steps to get the project up and running on your local machine.PrerequisitesNode.js (v18 or higher): Download Node.jsnpm (Node Package Manager) or YarnModern web browserCode editor (VS Code recommended)InstallationAccept the GitHub Classroom assignment invitation and clone your personal repository to your local machine.git clone your-repository-url
cd your-repository-name
Install project dependencies:npm install
# or yarn install
Running the ApplicationStart the development server:npm run dev
# or yarn dev
Open your web browser and navigate to the address provided in your terminal (usually http://localhost:5173/).Project StructureThe application adheres to a standard React project structure, promoting modularity and maintainability:src/
├── components/         # Reusable UI components (e.g., Header, PostCard)
├── pages/              # Page-level components (not explicitly used in this single-page example but good practice)
├── hooks/              # Custom React hooks (not explicitly used in this example but reserved)
├── context/            # React context providers (not explicitly used in this example but reserved)
├── api/                # API integration functions (conceptual, logic is in App.jsx for this example)
├── utils/              # Utility functions (not explicitly used in this example but reserved)
└── App.jsx             # Main application component
API IntegrationThe application integrates with the JSONPlaceholder API to fetch sample post data.Endpoint: https://jsonplaceholder.typicode.com/postsMethod: GETImplementation: Data fetching is handled within the App.jsx component using the fetch API and useEffect/useCallback hooks to manage loading and error states.Deployment(Replace this section with your actual deployment details once you've deployed your app.)This application has been deployed to a public hosting service. You can view the live application here:Deployment URL: [YOUR_DEPLOYMENT_URL_HERE](Example deployment platforms: Vercel, Netlify, GitHub Pages. You'll need to follow their specific instructions to deploy your Vite React app.)SubmissionYour work will be automatically submitted when you push your changes to your GitHub Classroom repository. Please ensure you have:Completed all required components and features.Implemented proper state management with React hooks.Integrated with at least one external API.Styled your application with Tailwind CSS.Deployed your application and added the deployment URL to this README.md file.
