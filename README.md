# searchfuncreact
import React, { useState, useEffect } from 'react';

function App() {
  const [posts, setPosts] = useState([]);
  const [filteredPosts, setFilteredPosts] = useState([]);
  const [searchTerm, setSearchTerm] = useState('');

  // Fetch posts when component mounts
  useEffect(() => {
    const fetchPosts = async () => {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/posts');
        const data = await response.json();
        setPosts(data);
      } catch (error) {
        console.error('Error fetching posts:', error);
      }
    };

    fetchPosts();
  }, []);

  // Handle search input changes
  const handleSearchChange = (e) => {
    const value = e.target.value;
    setSearchTerm(value);

    // Filter posts based on title or body
    const filtered = posts.filter(post => 
      post.title.toLowerCase().includes(value.toLowerCase()) ||
      post.body.toLowerCase().includes(value.toLowerCase())
    );

    setFilteredPosts(filtered);
  };

  return (
    <div style={{
      fontFamily: 'Arial, sans-serif',
      maxWidth: '800px',
      margin: '0 auto',
      backgroundColor: 'white',
      padding: '20px',
      borderRadius: '8px',
      boxShadow: '0 2px 5px rgba(0,0,0,0.1)'
    }}>
      <div style={{marginBottom: '20px'}}>
        <input 
          type="text" 
          placeholder="Search posts..." 
          value={searchTerm}
          onChange={handleSearchChange}
          style={{
            width: '100%',
            padding: '10px',
            fontSize: '16px',
            border: '1px solid #ddd',
            borderRadius: '4px'
          }}
        />
      </div>

      <div style={{
        display: 'grid',
        gridGap: '15px'
      }}>
        {filteredPosts.length > 0 ? (
          filteredPosts.map(post => (
            <div 
              key={post.id} 
              style={{
                backgroundColor: '#f9f9f9',
                border: '1px solid #e0e0e0',
                borderRadius: '6px',
                padding: '15px',
                transition: 'box-shadow 0.3s ease'
              }}
              onMouseOver={(e) => e.currentTarget.style.boxShadow = '0 4px 6px rgba(0,0,0,0.1)'}
              onMouseOut={(e) => e.currentTarget.style.boxShadow = 'none'}
            >
              <h3 style={{marginTop: 0, color: '#333'}}>{post.title}</h3>
              <p style={{color: '#666', marginBottom: '10px'}}>{post.body}</p>
              <span style={{
                fontSize: '0.8em',
                color: '#888',
                display: 'block',
                textAlign: 'right'
              }}>
                Post ID: {post.id}
              </span>
            </div>
          ))
        ) : (
          <p style={{
            textAlign: 'center',
            color: '#888',
            padding: '20px'
          }}>
            {searchTerm ? 'No posts found' : 'Start typing to search posts'}
          </p>
        )}
      </div>
    </div>
  );
}

export default App;
