```html
<div class="map__init map-container js_map" id="map" 
    data-image="assets/front/img/baloon.png"            
    data-center="55.863739,37.446305"
    data-location="55.863899, 37.463341"
    data-title="Ауди Центр Север"
    data-zoom="14">
</div>
```
```js
var map = {
    create: function(params) {
        return ymaps.ready(function() {
            var map = new ymaps.Map(params.id, {
                center: params.center,
                zoom: params.zoom,
                controls: ['zoomControl'],
                // controls: [],
                suppressMapOpenBlock: true,
                scroll: true
            }, {
                searchControlProvider: 'yandex#search'
            });

            map.geoObjects.add(new ymaps.Placemark(params.location, {
                hintContent: params.title,
            }, {
                iconLayout: 'default#imageWithContent',
                // Своё изображение иконки метки.
                iconImageHref: params.image,
                // Размеры метки.
                iconImageSize: [52, 76],
                // Смещение левого верхнего угла иконки относительно
                // её "ножки" (точки привязки).
                iconImageOffset: [-24, -54],
                // Смещение слоя с содержимым относительно слоя с картинкой.
                iconContentOffset: [15, 15],
            }));

            // map.behaviors.disable(['scrollZoom']);
        });
    },
    init: function() {
        if ($('.js_map').length) {
            var block = $('.js_map');
            map.create({
                id: block.attr('id'),
                center: block.data('center').split(','),
                location: block.data('location').split(','),
                title: block.data('title'),
                image: block.data('image'),
                zoom: block.data('zoom'),
            });
        }
    },
};
```
