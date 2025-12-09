<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Reddit Clone</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background-color: #dae0e6;
      padding-top: 60px;
    }

    .header {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      background: white;
      border-bottom: 1px solid #ccc;
      padding: 12px 20px;
      z-index: 100;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }

    .header h1 {
      font-size: 24px;
      color: #ff4500;
      font-weight: bold;
    }

    .container {
      max-width: 640px;
      margin: 0 auto;
      padding: 20px;
    }

    .post {
      background: white;
      border: 1px solid #ccc;
      border-radius: 4px;
      margin-bottom: 10px;
      display: flex;
      overflow: hidden;
      transition: border-color 0.2s;
      position: relative;
    }

    .post:hover {
      border-color: #888;
    }

    .vote-section {
      background: #f8f9fa;
      padding: 8px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 4px;
      min-width: 40px;
    }

    .vote-btn {
      background: none;
      border: none;
      cursor: pointer;
      color: #878a8c;
      padding: 2px;
      border-radius: 2px;
      transition: all 0.2s;
    }

    .vote-btn:hover {
      background: #e9ecef;
    }

    .vote-btn.upvoted {
      color: #ff4500;
    }

    .vote-btn.downvoted {
      color: #7193ff;
    }

    .vote-count {
      font-size: 12px;
      font-weight: bold;
      color: #1c1c1c;
    }

    .vote-count.upvoted {
      color: #ff4500;
    }

    .vote-count.downvoted {
      color: #7193ff;
    }

    .post-content {
      flex: 1;
      padding: 12px;
      position: relative;
    }

    .post-header {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 12px;
      color: #787c7e;
      margin-bottom: 8px;
    }

    .subreddit {
      font-weight: bold;
      color: #1c1c1c;
    }

    .post-title {
      font-size: 18px;
      font-weight: 500;
      color: #1c1c1c;
      margin-bottom: 8px;
      line-height: 1.3;
    }

    .post-text {
      font-size: 14px;
      color: #1c1c1c;
      margin-bottom: 8px;
      line-height: 1.5;
    }

    .post-image {
      width: 100%;
      max-height: 400px;
      object-fit: cover;
      margin-bottom: 8px;
      border-radius: 4px;
    }

    .post-actions {
      display: flex;
      gap: 8px;
      font-size: 12px;
      font-weight: bold;
      color: #878a8c;
    }

    .action-btn {
      background: none;
      border: none;
      cursor: pointer;
      padding: 6px 8px;
      border-radius: 2px;
      display: flex;
      align-items: center;
      gap: 6px;
      color: #878a8c;
      font-weight: bold;
      transition: background 0.2s;
    }

    .action-btn:hover {
      background: #f6f7f8;
    }

    .loading {
      text-align: center;
      padding: 20px;
      color: #878a8c;
    }

    svg {
      width: 20px;
      height: 20px;
    }

    .action-btn svg {
      width: 16px;
      height: 16px;
    }

    .ai-flag {
      position: absolute;
      top: 8px;
      right: 8px;
      background: #a7a9b1
      border: none;
      border-radius: 4px;
      padding: 6px 10px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 6px;
      color: white;
      font-size: 11px;
      font-weight: bold;
      transition: all 0.2s;
      z-index: 10;
    }

    .ai-flag:hover {
      transform: translateY(-2px);
    }

    .ai-flag svg {
      width: 14px;
      height: 14px;
    }

    .ai-tooltip {
      position: absolute;
      top: 42px;
      right: 8px;
      background: white;
      border: 1px solid #e0e0e0;
      border-radius: 8px;
      padding: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
      z-index: 20;
      min-width: 200px;
      display: none;
    }

    .ai-tooltip.show {
      display: block;
      animation: fadeIn 0.2s;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(-5px); }
      to { opacity: 1; transform: translateY(0); }
    }

    .ai-tooltip-header {
      font-weight: bold;
      margin-bottom: 8px;
      color: #1c1c1c;
      font-size: 13px;
    }

    .ai-detection-item {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 6px 0;
      font-size: 12px;
      color: #555;
    }

    .ai-detection-item svg {
      width: 16px;
      height: 16px;
    }

    .ai-badge {
      background: black;
      color: white;
      padding: 2px 6px;
      border-radius: 3px;
      font-size: 10px;
      font-weight: bold;
    }

    .highlight-btn {
      margin-top: 8px;
      padding: 8px 12px;
      background: black;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-size: 12px;
      font-weight: bold;
      width: 100%;
      transition: background 0.2s;
    }

    .highlight-btn:hover {
      background: #764ba2;
    }

    .highlight-btn.active {
      background: #ff4500;
    }

    .ai-highlight {
      background: linear-gradient(120deg, #fef08a 0%, #fde047 100%);
      padding: 2px 4px;
      border-radius: 2px;
    }
  </style>
</head>
<body>
  <div class="header">
    <h1>readdit</h1>
  </div>

  <div class="container" id="posts-container"></div>
  <div class="loading" id="loading">Loading posts...</div>

  <script>
    let postId = 0;
    let posts = [];
    let postsData = [];
    let currentIndex = 0;

    // Load posts from JSON file
    fetch('src/posts.json')
      .then(response => response.json())
      .then(data => {
        postsData = data;
        loadMorePosts();
      })
      .catch(error => {
        console.error('Error loading posts:', error);
        document.getElementById('loading').textContent = 'Error loading posts. Make sure posts.json exists!';
      });

    function formatVotes(votes) {
      if (votes >= 1000) {
        return (votes / 1000).toFixed(1) + 'k';
      }
      return votes.toString();
    }

    function vote(postId, direction) {
      const post = posts.find(p => p.id === postId);
      if (!post) return;

      const oldVote = post.userVote;
      const newVote = oldVote === direction ? 0 : direction;
      post.votes += (newVote - oldVote);
      post.userVote = newVote;

      updatePost(postId);
    }

    function updatePost(postId) {
      const post = posts.find(p => p.id === postId);
      const el = document.querySelector(`[data-post-id="${postId}"]`);
      if (!el || !post) return;

      const voteCount = el.querySelector('.vote-count');
      const upBtn = el.querySelector('.upvote-btn');
      const downBtn = el.querySelector('.downvote-btn');

      voteCount.textContent = formatVotes(post.votes);
      voteCount.className = 'vote-count';
      upBtn.className = 'vote-btn upvote-btn';
      downBtn.className = 'vote-btn downvote-btn';

      if (post.userVote === 1) {
        voteCount.classList.add('upvoted');
        upBtn.classList.add('upvoted');
      } else if (post.userVote === -1) {
        voteCount.classList.add('downvoted');
        downBtn.classList.add('downvoted');
      }
    }

    function toggleAITooltip(postId) {
      const tooltip = document.querySelector(`[data-tooltip-id="${postId}"]`);
      if (tooltip) {
        tooltip.classList.toggle('show');
      }
    }

    function toggleHighlight(postId) {
      const post = posts.find(p => p.id === postId);
      const postEl = document.querySelector(`[data-post-id="${postId}"]`);
      const btn = postEl.querySelector('.highlight-btn');

      if (!post.highlighted) {
        // Highlight AI text
        if (post.aiTypes.includes('text')) {
          const titleEl = postEl.querySelector('.post-title');
          const contentEl = postEl.querySelector('.post-text');

          if (titleEl) {
            const text = titleEl.textContent;
            titleEl.innerHTML = `<span class="ai-highlight">${text}</span>`;
          }
          if (contentEl) {
            const text = contentEl.textContent;
            contentEl.innerHTML = `<span class="ai-highlight">${text}</span>`;
          }
        }

        // Highlight AI image
        if (post.aiTypes.includes('image')) {
          const imgEl = postEl.querySelector('.post-image');
          if (imgEl) {
            imgEl.style.border = '4px solid #fde047';
            imgEl.style.boxShadow = '0 0 0 4px rgba(251, 191, 36, 0.3)';
          }
        }

        post.highlighted = true;
        btn.classList.add('active');
        btn.textContent = 'Remove Highlights';
      } else {
        // Remove highlights
        if (post.aiTypes.includes('text')) {
          const titleEl = postEl.querySelector('.post-title');
          const contentEl = postEl.querySelector('.post-text');

          if (titleEl) {
            const span = titleEl.querySelector('.ai-highlight');
            if (span) titleEl.textContent = span.textContent;
          }
          if (contentEl) {
            const span = contentEl.querySelector('.ai-highlight');
            if (span) contentEl.textContent = span.textContent;
          }
        }

        if (post.aiTypes.includes('image')) {
          const imgEl = postEl.querySelector('.post-image');
          if (imgEl) {
            imgEl.style.border = '';
            imgEl.style.boxShadow = '';
          }
        }

        post.highlighted = false;
        btn.classList.remove('active');
        btn.textContent = 'Highlight AI Content';
      }
    }

    document.addEventListener('click', (e) => {
      if (!e.target.closest('.ai-flag') && !e.target.closest('.ai-tooltip')) {
        document.querySelectorAll('.ai-tooltip').forEach(t => t.classList.remove('show'));
      }
    });

    function createPostElement(post) {
      const div = document.createElement('div');
      div.className = 'post';
      div.setAttribute('data-post-id', post.id);

      const hasAI = post.aiTypes && post.aiTypes.length > 0;
      const aiFlag = hasAI ? `
        <button class="ai-flag" onclick="toggleAITooltip(${post.id})">
          <svg fill="currentColor" viewBox="0 0 24 24">
            <path d="M9 3L5 7v6l4 4 4-4V7L9 3zm0 2.83L10.17 7 9 8.17 7.83 7 9 5.83zM5 9h8v4H5V9zm7.5 11.5l-1.09-1.09L12.5 18l-1.09-1.41L10.5 17l2 2.5 3.5-4.5-1.41-1.09-2.59 3.34-1-1.25z"/>
          </svg>
          AI
        </button>
        <div class="ai-tooltip" data-tooltip-id="${post.id}">
          <div class="ai-tooltip-header">🤖 Dave Says...</div>
          ${post.aiTypes.includes('text') ? `
            <div class="ai-detection-item">
              <svg fill="currentColor" viewBox="0 0 24 24">
                <path d="M3 17.25V21h3.75L17.81 9.94l-3.75-3.75L3 17.25zM20.71 7.04c.39-.39.39-1.02 0-1.41l-2.34-2.34c-.39-.39-1.02-.39-1.41 0l-1.83 1.83 3.75 3.75 1.83-1.83z"/>
              </svg>
              <span><span class="ai-badge">TEXT</span> Content generated by AI</span>
            </div>
          ` : ''}
          ${post.aiTypes.includes('image') ? `
            <div class="ai-detection-item">
              <svg fill="currentColor" viewBox="0 0 24 24">
                <path d="M21 19V5c0-1.1-.9-2-2-2H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2zM8.5 13.5l2.5 3.01L14.5 12l4.5 6H5l3.5-4.5z"/>
              </svg>
              <span><span class="ai-badge">IMAGE</span> Image created by AI</span>
            </div>
          ` : ''}
          <button class="highlight-btn" onclick="toggleHighlight(${post.id})">
            Highlight AI Content
          </button>
        </div>
      ` : '';

      div.innerHTML = `
        <div class="vote-section">
          <button class="vote-btn upvote-btn" onclick="vote(${post.id}, 1)">
            <svg fill="currentColor" viewBox="0 0 24 24"><path d="M12 4l8 8h-6v8h-4v-8H4z"/></svg>
          </button>
          <span class="vote-count">${formatVotes(post.votes)}</span>
          <button class="vote-btn downvote-btn" onclick="vote(${post.id}, -1)">
            <svg fill="currentColor" viewBox="0 0 24 24"><path d="M12 20l-8-8h6V4h4v8h6z"/></svg>
          </button>
        </div>
        <div class="post-content">
          ${aiFlag}
          <div class="post-header">
            <span class="subreddit">r/${post.subreddit}</span>
            <span>•</span>
            <span>Posted by u/${post.author}</span>
            <span>•</span>
            <span>${post.timeAgo}</span>
          </div>
          <div class="post-title">${post.title}</div>
          ${post.content ? `<div class="post-text">${post.content}</div>` : ''}
          ${post.hasImage ? `<img class="post-image" src="https://picsum.photos/600/400?random=${post.id}" alt="Post image">` : ''}
          <div class="post-actions">
            <button class="action-btn">
              <svg fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"/></svg>
              ${post.comments} Comments
            </button>
            <button class="action-btn">
              <svg fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.368 2.684 3 3 0 00-5.368-2.684z"/></svg>
              Share
            </button>
            <button class="action-btn">
              <svg fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 5a2 2 0 012-2h10a2 2 0 012 2v16l-7-3.5L5 21V5z"/></svg>
              Save
            </button>
          </div>
        </div>
      `;

      return div;
    }

    function loadMorePosts() {
      const container = document.getElementById('posts-container');
      const batchSize = 5;

      for (let i = 0; i < batchSize && currentIndex < postsData.length; i++) {
        const postData = postsData[currentIndex];
        const post = {
          id: postId++,
          ...postData,
          userVote: 0,
          highlighted: false
        };

        posts.push(post);
        container.appendChild(createPostElement(post));
        currentIndex++;
      }

      if (currentIndex >= postsData.length) {
        document.getElementById('loading').textContent = 'No more posts to load';
      }
    }

    const observer = new IntersectionObserver((entries) => {
      if (entries[0].isIntersecting && currentIndex < postsData.length) {
        loadMorePosts();
      }
    }, { threshold: 0.1 });

    observer.observe(document.getElementById('loading'));
  </script>
</body>
</html>