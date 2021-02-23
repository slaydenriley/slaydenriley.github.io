---
layout: post
title:      "Implementing Quill Editor in a Rails/React/Redux project "
date:       2021-02-23 21:19:01 +0000
permalink:  implementing_quill_editor_in_a_rails_react_redux_project
---


For my final portfolio project for Flatiron School, I set out to create a portfolio website for myself--a place where I could showcase my projects and blog posts.

What started out as a fairly basic project turned into something much more complex as I continued adding features and functionailties to my portfolio.

One of those features was a full admin panel for myself, which would allow me to create new posts and projects, view and manage users, create new tags, and ultimately be able to edit and delete all of the above.

These admin features just involved some CRUD, so I thought it would be fairly simple! However, when I started thinking about my post editor, things got a lot more complex. Up until this point, all of my "text editors" in projects included just a text input or textarea. But wait: how does the Wordpress or Medium editor allow you to *style* your posts before publishing, and have them render correctly on the DOM? I never thought about that!

This is when I learned about **rich text editors**, which allow you to make a WYSIWYG ("what you see is what you get") text input that appears on the DOM exactly as you style it in the editor.

Whoa, cool! But how? For this project, I am using a Rails API on the backend and React/Redux on the frontend.

## Rich Text Editors

Working with Rails, there are several ways to implement this. I found the simpliest way was to **save text as an HTML string in the database, and then render that HTML to the dom.** Sounds easy enough. The first rich text editor I stumbled upon was Draft JS, created by Facebook (who conviently created React as well). Draft JS is actually a full framework for people wanting to build their own rich text editors.

However, I found Draft to be quite annoying and buggy because of the sheer amount of OPTIONS and work that went into building a custom editor. I also found the documentation confusing and thus decided to keep searching for something a bit more ready-to-go, out of the box. Draft JS would be a great option if you needed some very customized and specific features.

## Enter, the Quill Editor

Once finding Quill, things got a lot better for me. It wasn't as buggy. It was simple, easy to use, and provided all of the tweaks and customizations I needed to create a nice text editor. You can check out Quill Editor here: https://quilljs.com/.

Here's how I used it in my NewPostContainer component (and implemented similarily in my EditPost component):

```
import React from 'react'
import NewPost from '../../components/posts/NewPost'
import {connect} from 'react-redux'
import addNewPost from '../../actions/addNewPost.js'
import fetchTags from '../../actions/fetchTags.js'
import ReactQuill from 'react-quill';
import { BlockReserveLoading } from 'react-loadingg';
import 'react-quill/dist/quill.snow.css';

class NewPostContainer extends React.Component {
  state = {
    title: '',
    user_id: this.props.user_id,
    content: '',
    category: "post",
    redirectToNewPage: false,
    image_link: '',
    tags: []
  };

  componentDidMount() {
    this.props.fetchTags()
  }

  handleChange = event => {
    let index
    if (event.target.type === "checkbox" && event.target.checked === true) {
      this.state.tags.push(event.target.value)
    } else if (event.target.type === "checkbox" && event.target.checked === false) {
        index = this.state.tags.indexOf(+event.target.value)
        this.state.tags.splice(index, 1)
    } else {
        this.setState({
          [event.target.name]: event.target.value
        })
      }
  };

  handleSubmit = (event) => {
    event.preventDefault()
    const formData = this.state
    this.props.addNewPost(formData)
    this.props.history.push(`/${this.state.category}s`)
  };

  handleEditorChange = (value) => {
    this.setState({content: value})
  }

  modules = {
    toolbar: [
      [{ 'header': [1, 2, false] }],
      ['bold', 'italic', 'underline','strike', 'blockquote', 'code-block'],
      [{'list': 'ordered'}, {'list': 'bullet'}, {'indent': '-1'}, {'indent': '+1'}],
      ['link', 'image', 'video'],
      ['clean']
    ],
  }

  handleLoading = () => {
    if (this.props.tags.requesting) {
      return <BlockReserveLoading />;
    }
    else {
      return (
        <div className="new-post">
          <div onSubmit={this.handleSubmit} onChange={this.handleChange}>
            <NewPost tags={this.props.tags.tags}/>
          </div>

            <div className="rich-text-editor">
              <ReactQuill
                value={this.state.content}
                onChange={this.handleEditorChange}
                modules={this.modules}
              />
            </div>
        </div>
      )
    }
  }

  render() {
    return (
      <>{this.handleLoading()}</>
    )
  }
}

const mapStateToProps = (state) => {
    return {
        user_id: state.account.user.id,
        tags: state.tags,
        post: state.single_post
    };
};

export default connect(mapStateToProps, {addNewPost, fetchTags})(NewPostContainer)
```

As you can see, I imported Quill editor and rendered it in my handle loading function. To customize the toolbar, I created a modules object, which allows for toolbar customization.

Under the handleEditorChange hook, we can see that the editor content gets set to state.content, which then will be saved to the database as a string of HTML. The "value" of the post editor is the state.content as well, making this a controlled component.

## Rendering an HTML String

In my PostShow.js component where I need to render the post content, I have the following:

```
const ShowPost = props => {
  return (
    <div className="single-post">
      <div className="center">
        <h1 className="post-title">{props.post.title}</h1>
        <em className="created_at">Published on: {props.post.created_at}</em>
        <hr className="line"/>
        <img alt="Featured" src={props.post.image_link}/>
      </div>

      <div className="post-content" dangerouslySetInnerHTML={{ __html: props.post.content}} />

      </div>
  )
};

export default ShowPost;
```

Oh uh, that looks scary! If you're wanting to learn more about dangerouslySetInnerHTML, here is a great article explaining what it is an what it does: https://medium.com/better-programming/what-is-dangerouslysetinnerhtml-6d6a98cbc187. In short, it's React's way of setting innerHTML.

And there you go! That's a (very) short summary of how I used Quill editor in my portfolio website. Here's a shot of what it looks like from my end:

![](https://i.imgur.com/bBiQ65N.png)

Feel free to send me questions if you're stuck on this. Overall, it just takes some playing around and tweaking to get this to work, and while it may seem confusing at first, you'll figure it out!


