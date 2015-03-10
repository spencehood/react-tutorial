# Comment

```javascript
var converter = new Showdown.converter();
var Comment = React.createClass({
  render: function() {
    var rawMarkup = converter.makeHtml(this.props.children.toString());
    return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        <span dangerouslySetInnerHTML={{__html: rawMarkup}} />
      </div>
    );
  }
});
```
### Q1: Where does the value of ``this.props.author`` get specified?

Within the 'Comment' variable, 'this.props.author' gets displayed. The VALUE of 'this.props.author', however gets set within the 'CommentForm' variable. In other words, once a user makes a comment, whatever is in the 'Your name' field gets stored in the JSON data object as 'this.props.author'.

### Q2: Where does the value of ``this.props.children`` get specified?

{{ your answer here }}


### Q3: What does ``className="comment"`` do?

{{ your answer here }}

### Q4: What is ``dangerouslySetInnerHTML``? Why is it such a long word for an API method?

{{ your answer here }}

# CommentBox
```javascript
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },

```

### Q5: How does ``$`` get defined? Is it part of the ReactJS framework?

{{your answer here}}

### Q6: Where does the value of ``this.props.url`` get specified?

This url gets specified at the bottom of the file in the React.render() function. For our purposes, it is set as the 'comments.json file' within our own directory, but - more realistically - this would be set to some url online where the data we are fetching would be hosted. Once specified, 'this.props.url' in the 'CommentBox' variable refers to that unchanging trait of the variable itself - in this case the data url. 


### Q7: What would happen to the statement ``this.setState`` if ``bind(this)`` is removed? Why?

{{your answer here}}

### Q8: Who calls ``loadCommentsFromServer``? When? 

{{your answer here}}


```javascript
	
  handleCommentSubmit: function(comment) {
    var comments = this.state.data;
    comments.push(comment);
    this.setState({data: comments}, function() {
      // `setState` accepts a callback. To avoid (improbable) race condition,
      // `we'll send the ajax request right after we optimistically set the new
      // `state.
      $.ajax({
        url: this.props.url,
        dataType: 'json',
        type: 'POST',
        data: comment,
        success: function(data) {
          this.setState({data: data});
        }.bind(this),
        error: function(xhr, status, err) {
          console.error(this.props.url, status, err.toString());
        }.bind(this)
      });
    });
  },
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
      </div>
    );
  }
});
```

### Q9: What is the purpose of ``this.state``? How is this different from ``this.props``?

{{your answer here}}

### Q10: What is the initial value of ``this.state.data``? How is the initial value specified?

{{your answer here}}


### Q11: What is the new value of ``this.state.data``? How is this new value set?

The new value of 'this.state.data' includes all of the comment data from before, plus whatever new data was just fetched from the server. This new value is only ever set when we have a  'this.setState({data: data})' line. This says if the request for data from the server was successful, set 'this.state.data' to the new data value(s) from that server call.


### Q12: What is the purpose of ``componentDidMount`` callback?

{{your answer here}}

### Q13: What is the purpose of ``getInitialState``?

{{your answer here}}


# CommentList

```javascript

var CommentList = React.createClass({
  render: function() {
    var commentNodes = this.props.data.map(function(comment, index) {
      return (
        // `key` is a React-specific concept and is not mandatory for the
        // purpose of this tutorial. if you're curious, see more here:
        // http://facebook.github.io/react/docs/multiple-components.html#dynamic-children
        <Comment author={comment.author} key={index}>
          {comment.text}
        </Comment>
      );
    });
    return (
      <div className="commentList">
        {commentNodes}
      </div>
    );
  }
});
```
### Q14: How does the value of ``this.props.data`` get set?

{{your answer here}}

### Q15: What is the value of ``commentNodes``?

{{your answer here}}

### Q16: Where does the value of ``{comment.text}`` go on the rendered page?

The value of '{comment.text}' goes right below the author name and horizontal rule on the rendered page. We see it embedded within the <Comment> tag in that 'CommentList' variable, and it was set with the 'ref="text"' attribtute on the input tag within the 'CommentForm' variable.

# CommentForm
```javascript

var CommentForm = React.createClass({
  handleSubmit: function(e) {
    e.preventDefault();
    var author = this.refs.author.getDOMNode().value.trim();
    var text = this.refs.text.getDOMNode().value.trim();
    if (!text || !author) {
      return;
    }
    this.props.onCommentSubmit({author: author, text: text});
    this.refs.author.getDOMNode().value = '';
    this.refs.text.getDOMNode().value = '';
  },
  render: function() {
    return (
      <form className="commentForm" onSubmit={this.handleSubmit}>
        <input type="text" placeholder="Your name" ref="author" />
        <input type="text" placeholder="Say something..." ref="text" />
        <input type="submit" value="Post" />
      </form>
    );
  }
});

React.render(
  <CommentBox url="comments.json" pollInterval={2000} />,
  document.getElementById('content')
);
```

### Q17: What is the purpose of ``e.preventDefault()``?

{{answer here}}

### Q18: What is the value of ``this.props.onCommentSubmit``? What does it get specified?

{{answer here}}

### Q19: Where does ``this.refs.author`` point to?

{{answer here}}

### Q20: What does ``getDOMNode()`` do?

{{answer here}}