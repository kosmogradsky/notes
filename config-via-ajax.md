# Меняем конфиг проекта без пересборки

Пришёл заказчик и попросил сделать конфиг, в котором он сможет сам прописывать адрес бекенда и специальные хедеры, которые нужно отправлять с каждым запросом. Сразу в голове всплывают два решения:
* Использовать [DefinePlugin](https://webpack.js.org/plugins/define-plugin/). Но этот вариант нам не подходит, потому что такой конфиг тесно связан с конфигом Вебпака. Заказчику будет сложно его редактировать.
* Использовать [DotenvPlugin](https://github.com/mrsteele/dotenv-webpack). Но этот вариант нам тоже не подходит, потому что после каждого изменения конфига заказчику придётся пересобирать проект. Это неудобно, и это надо объяснять.

В итоге мы решили держать конфиг в JSON-файле и подгружать его через AJAX при старте приложения.

Конфиг выглядит примерно так:
```json
{
  "apiUrl": "https://evil.com/api/v1",
  "apiHeaders": {
    "Api-Header1": "someusefuldata",
    "Api-Header2": "moreofsomeusefuldata"
  }
}
```

Загрузка конфига на старте упрощённо выглядит так:
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

Разумеется, можно использовать другой фреймворк или сохранять данные не в состояние компонента, а, например, в Redux.