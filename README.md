# Backbone 정리

### 개념
- Backbone.js은 클라이언트 사이드 웹애플리케이션 개발에 MVC패턴 적용을 가능하게 해주는 자바스크립트 프레임워크들 중 하나
-  MVC패턴에 맞게 개발을 할 수 있습니다. 즉, M(model, 데이터), V(view, UI), C(controller, 로직, 데이터 처리)로 코드의 역할을 나눠서 작성하고 관리할 수 있게 해줍니다. 
- Models : MVC에서의 model입니다. 개별 데이터를 나타냅니다.
- Collections : model의 집합입니다. view와 연결되어, model에 변화가 생길 때 손쉽게 view(UI)를 갱신할 수 있습니다.
- Routers : MVC에서의 controller 입니다. location.hash의 변경에 따른 처리를 담당합니다.
- Views : MVC에서의 view입니다. 화면에 나타나는 UI를 담당하며 프론트엔드의 특성상 view가 controller 의 성격도 가지고 있습니다.

## VIEW

### EL(뷰의 DOM)
- 백본에서 모든 뷰는 DOM 을 가지고 있어서 언제나 렌더 가능하고 DOM에 삽입 가능하다
- tagName, className, id등 모든 것에 EL 적용 가능
- **$el**은 el을 캐시해서 참조하는 jquery 뷰 요소

```javascript
var ItemView = Backbone.View.extend({
  tagName: 'li'
});

var BodyView = Backbone.View.extend({
  el: 'body'
});

var item = new ItemView();
var body = new BodyView();

alert(item.el + ' ' + body.el);
```

### initialize
- model view 생성 초기에 attributes 설정
```javascript
new Book({
  title: "One Thousand and One Nights",
  author: "Scheherazade"
});
```

### render
- 필수, model data를 통해 view를 렌더링 
```javascript
var Bookmark = Backbone.View.extend({
  template: _.template(...),
  render: function() {
    this.$el.html(this.template(this.model.attributes));
    return this;
  }
});
```

## MODEL

### .parse(response, option)
- fetch, save 실행 시 서버에서 데이터 리턴되면 실행
- response를 전달받고 모델에 특정 해시를 넣어서 반환
- Json으로 전달

### .fetch([option])
- Backbone.sync에 위임해서 서버에서 가져온 attribute를 -> 모델의 상태에 병합
- jqXHR을 리턴 
- 모델에 데이터가 채워지지 않은 경우 / 최신 서버상태를 유지하려는 경우 유용
- 현재 attribute와 서버 상태가 다를 경우 “change” 이벤트 발생시킴
- 성공과 에러 콜백을 받을 수 있음
```javascript
setInterval(function() {
  channel.fetch();
}, 10000);
```

## EVENT

### object.trigger(event, args)
- event를 설정한 쪽에 콜백

### object.listenTo(other, event, callback)
- object에게 other에 있는 event를 감지하고 있게 설정
- **other.on**과 다른 점은 **listenTo**는 뷰를 제거하면 자동으로 이벤트 핸들러 제거
```javascript
view.listenTo(model, 'change', view.render);
```

