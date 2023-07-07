# Data Flow

In chapter 1, we learned how to create a counter app using Redux Toolkit. Now we will learn how to use Redux Toolkit to create a simple post app.

## Create the post slice

Create a new file called `postSlice.ts` inside `features` folder:

```ts
import { createSlice, PayloadAction } from "@reduxjs/toolkit";

// Define a type for the slice state
export interface PostState {
  posts: Post[];
  status: "idle" | "loading" | "failed";
}

// Define interface for a single post
export interface Post {
  id: string;
  title: string;
  content: string;
  user: string;
}

const initialState: PostState = {
  posts: [
    {
      id: "1",
      title: "First Post!",
      content: "Hello!",
      user: "0",
    },
    {
      id: "2",
      title: "Second Post",
      content: "More text",
      user: "0",
    },
  ],
  status: "idle",
};

export const postSlice = createSlice({
  name: "posts",
  initialState,
  reducers: {
    addPost: (state, action: PayloadAction<Post>) => {
      state.posts.push(action.payload);
    },
  },
});

export const { addPost } = postSlice.actions;

export default postSlice.reducer;
```

## Add the slice reducer to the store:

```ts
export const store = configureStore({
  reducer: {
    counter: counterReducer,
    posts: postReducer,
  },
});
```

## Create the post component

Create a new file called `Post.tsx` inside `features` folder:

```tsx
import { useAppSelector, useAppDispatch } from "../../app/hooks";

function PostsList() {
  const posts = useAppSelector((state) => state.posts.posts);
  const dispatch = useAppDispatch();

  return (
    <div>
      <h1>Posts</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}

export default PostsList;
```

## Add the post component to the app

```tsx
function App() {
  return <PostsList />;
}
```

## Add the add post form component

```tsx
import { SetStateAction, useState } from "react";
import { useAppDispatch } from "../../app/hooks";
import { addPost } from "./postsSlice";

function AddPostForm() {
  const [title, setTitle] = useState("");
  const [content, setContent] = useState("");

  const onTitleChanged = (e: { target: { value: SetStateAction<string> } }) =>
    setTitle(e.target.value);
  const onContentChanged = (e: { target: { value: SetStateAction<string> } }) =>
    setContent(e.target.value);

  const dispatch = useAppDispatch();

  const onSavePostClicked = () => {
    if (title && content) {
      dispatch(
        addPost({
          id: crypto.randomUUID(),
          title,
          content,
          user: "1",
        })
      );
      setTitle("");
      setContent("");
    }
  };

  return (
    <section>
      <h2>Add a New Post</h2>
      <form>
        <label htmlFor="postTitle">Post Title:</label>
        <input
          type="text"
          id="postTitle"
          name="postTitle"
          value={title}
          onChange={onTitleChanged}
        />
        <label htmlFor="postContent">Content:</label>
        <textarea
          id="postContent"
          name="postContent"
          value={content}
          onChange={onContentChanged}
        />
        <button type="button" onClick={onSavePostClicked}>
          Save Post
        </button>
      </form>
    </section>
  );
}

export default AddPostForm;
```

## Add the add post form component to the app

```tsx
function App() {
  return (
    <>
      <AddPostForm />
      <PostsList />
    </>
  );
}
```

Note how the new post is dispatched to the store including the `uuid`:

```ts
dispatch(
  addPost({
    id: crypto.randomUUID(),
    title,
    content,
    user: "1",
  })
);
```

This is always not a good idea. The `uuid` should be generated in the post slice reducer. In larger applications with multiple components dispatching actions, it is preferred to combine these actions in the slice reducer.

In post slice reducer, add the following:

```ts
export const postSlice = createSlice({
  name: "posts",
  initialState,
  reducers: {
    addPost: {
      reducer: (state, action: PayloadAction<Post>) => {
        state.posts.push(action.payload);
      },
      prepare: (title: string, content: string, user: string) => ({
        payload: {
          id: crypto.randomUUID(),
          title,
          content,
          user,
        },
      }),
    },
  },
});
```

Now, the `uuid` is generated in the slice reducer and the component dispatches the action as follows:

```ts
dispatch(addPost(title, content, "1"));
```
