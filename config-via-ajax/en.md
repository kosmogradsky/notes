# Change project config without rebuilding

The client comes and asks for a config file in which he can define backend URL and special headers which are required for every request. Two possible solutions come in mind instantly:
* Use [DefinePlugin](https://webpack.js.org/plugins/define-plugin/). This doesn't fit, because this config is coupled with Webpack config. Our client doesn't want to bother with that.
* Use [DotenvPlugin](https://github.com/mrsteele/dotenv-webpack). This doesn't fit either, because on every config change client would need to rebuild the project. This is inconvenient, and it must be explained.

In the end we decided to keep the config in a JSON file and load it via AJAX when the app starts. When the client needs it he can just open the file and edit it. The app will get new config on reload.

Here is how the config looks like:
```json
{
  "apiUrl": "https://evil.com/api/v1",
  "apiHeaders": {
    "Api-Header1": "someusefuldata",
    "Api-Header2": "moreofsomeusefuldata"
  }
}
```

Simplified, this is how config loading looks like:
```jsx
import React from 'react';
import axios from 'axios';

export default class PersonList extends React.Component {
  state = {
    config: undefined
  }

  componentDidMount() {
    axios.get(`/assets/config.json`)
      .then(res => {
        const config = res.data;
        this.setState({ config });
      })
  }

  render() {
    return config === undefined ? (
      <div>Загружаем конфиг...</div>
    ) : (
      <div>
        Конфиг успешно загружен!<br/>
        Адрес бекенда: {this.state.config.apiUrl}<br/>
        Теперь можно делать запросы.<br/>
      </div>
    )
  }
}
```

Obviously, you can use any other framework here or save loaded data not to the component state, but to Redux store, for example.
