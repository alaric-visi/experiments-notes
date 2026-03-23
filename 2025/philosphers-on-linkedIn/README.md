# Philosophers on LinkedIn

## Overview
This project is a static frontend webpage that provides a satirical mock-up of a LinkedIn feed populated by famous historical philosophers. The application uses HTML/CSS for layout and minimal client-side JavaScript to switch between different pre-defined posts.

## Core Mechanisms

### UI Navigation and State
The page presents a navigation bar with buttons representing different philosophers (e.g., Kierkegaard, Diderot, Barthes, Derrida). The JavaScript relies on a simple class-toggling function to hide and show the corresponding hardcoded HTML elements.

```javascript
function showPost(postIndex) {
    const posts = document.querySelectorAll('.linkedin-post');
    // Hide all posts
    posts.forEach(post => {
        post.classList.remove('active-post');
    });

    // Display the selected post by adding the active class
    document.getElementById(`post-${postIndex}`).classList.add('active-post');
}
```

### CSS Styling
The visual design mimics the familiar layout and color scheme of the LinkedIn user interface. It utilizes CSS Flexbox (`display: flex`) extensively to compose the profile headers, navigation bar, and post components identically to standard professional networking feeds. Variables and classes like `.linkedin-post`, `.profile-section`, and `.post-header` are defined to structure the mock content.
