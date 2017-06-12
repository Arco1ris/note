jsx 使用if else 渲染的四种方式

##### option 1 直接放在 render 里

```react
class HelloMessage extends React.Component {
  render (){
    let userMessage;
    if (this.props.loggedIn) {
      userMessage = (
        <span>
          <h2>{ `Welcome Back ${ this.props.name }` }</h2>
          <p>You can visit settings to reset your password</p>
        </span>
      )
    } else {
      userMessage = (
        <h2>Hey man! Sign in to see this section</h2>
      )
    }
    return(
      <div>
        <h1>My Super React App</h1>
        { userMessage }
      </div>
    )
  }
}
```

##### option 2 从 render 里拿出来放进函数里

```react
class HelloMessage extends React.Component {

  renderUserMessage(){
    if (this.props.loggedIn) {
      return (
        <span>
          <h2>{ `Welcome Back ${ this.props.name }` }</h2>
          <p>You can visit settings to reset your password</p>
        </span>
      );
    } else {
      return (
        <h2>Hey man! Log in to see this section</h2>
      );
    }
  }

  render (){        
    return(
      <div>
        <h1>My Super React App</h1>
        { this.renderUserMessage() }
      </div>
    )
  }
}
```

##### option 3 只有文本的区别时，采用三目运算符

```react
class HelloMessage extends React.Component {
  render (){        
    return(
      <div>
        <h1>
          { this.props.loggedIn ?  'You are logged In' : 'You are not logged In' }
        </h1>
      </div>
    )
  }
}
```

##### option 4 在 render 中使用三目运算，但是结构不太清晰

```react
class HelloMessage extends React.Component {
  render (){
    return(
      <div>
        <h1>My Super React App</h1>
        { this.props.loggedIn ?
            <span>
              <h2>{ `Welcome Back ${ this.props.name }` }</h2>
              <p>You can visit settings to reset your password</p>
            </span>
            :
            <h2>Hey man! Log in to see this section</h2>
        }
      </div>
    )
  }
}
```