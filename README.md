# **Blog Website**

This is a full-featured **Blog Website** built with **React**, allowing users to sign up, login, add, edit, delete, and view posts. The application uses **Redux Toolkit** for state management, **React Router DOM** for routing, and **Appwrite** for backend services.

## **Features:**
- ✅ **User Authentication**: Sign up, login, and logout functionalities.
- 📝 **Add Blog Posts**: Create new blog entries with rich text content and featured images.
- ✏️ **Edit Blog Posts**: Modify existing posts.
- ❌ **Delete Blog Posts**: Remove posts.
- 🗃️ **Image Upload**: Upload and preview images using Appwrite Storage.
- ⚡ **Protected Routes**: Certain routes are accessible only to authenticated users.
- 🎨 **Responsive UI**: Styled with Tailwind CSS.

## **Technologies Used:**
- 🟢 **React** – Frontend framework.
- 🔄 **Redux Toolkit** – For global state management.
- 🌐 **React Router DOM** – For routing.
- 📂 **Appwrite** – Backend for auth, database, and file storage.
- 🎨 **Tailwind CSS** – For styling.
- ✒️ **TinyMCE** – Rich text editor for post content.

## **Important Functions & Code Snippets:**

### **1. Authentication Service (auth.js)**
#### Create account
```javascript
async createAccount({email, password, name}) {
    const userAccount = await this.account.create(ID.unique(), email, password, name);
    return userAccount ? this.login({email, password}) : userAccount;
}
```
#### Login
```javascript
async login({email, password}) {
    return await this.account.createEmailSession(email, password);
}
```
#### Get Current User
```javascript
async getCurrentUser() {
    try {
        return await this.account.get();
    } catch (error) {
        console.error("Error:", error);
        return null;
    }
}
```

### **2. Post Management (config.js)**
#### Create Post
```javascript
async createPost({title, slug, content, featuredImage, status, userId}) {
    return await this.databases.createDocument(
        conf.appwriteDatabaseId,
        conf.appwriteCollectionId,
        slug,
        { title, content, featuredImage, status, userId }
    );
}
```
#### Get Posts
```javascript
async getPosts(queries = [Query.equal("status", "active")]) {
    return await this.databases.listDocuments(
        conf.appwriteDatabaseId,
        conf.appwriteCollectionId,
        queries
    );
}
```
#### Delete Post
```javascript
async deletePost(slug) {
    await this.databases.deleteDocument(conf.appwriteDatabaseId, conf.appwriteCollectionId, slug);
    return true;
}
```

### **3. Protected Route Wrapper (AuthLayout.jsx)**
```javascript
if(authentication && authStatus !== authentication){
    navigate("/login")
} else if(!authentication && authStatus !== authentication){
    navigate("/")
}
```

### **4. Post Form Slug Transformation**
```javascript
const slugTransform = useCallback((value) =>
    value.trim().toLowerCase().replace(/[^a-zA-Z\d\s]+/g, "-").replace(/\s/g, "-"),
[]);
```

## **Components in the Project:**
- **App.jsx**: Main layout and auth initialization.
- **Home.jsx**: Displays public posts.
- **Login.jsx & Signup.jsx**: Auth pages.
- **AllPosts.jsx**: Displays all posts for authenticated users.
- **AddPost.jsx & EditPost.jsx**: Forms to add or edit posts.
- **Post.jsx**: View single post with edit/delete options.
- **PostForm.jsx**: Reusable form for adding/editing posts.
- **PostCard.jsx**: Card UI to show post previews.
- **Header & Footer Components**: Common layouts.

## **Project Structure:**
```
├── appwrite
├── components
├── pages
├── store
├── App.jsx
├── main.jsx
├── conf.js
├── index.css
```

### Create `.env` file:
```
VITE_APPWRITE_URL=your-appwrite-url
VITE_APPWRITE_PROJECT_ID=your-project-id
VITE_APPWRITE_DATABASE_ID=your-database-id
VITE_APPWRITE_COLLECTION_ID=your-collection-id
VITE_APPWRITE_BUCKET_ID=your-bucket-id
VITE_TINYMCE_API_KEY=your-tinymce-api-key
```
