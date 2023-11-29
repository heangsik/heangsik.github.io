# AdorableCSS

## 목차

- [AdorableCSS](#adorablecss)
  - [목차](#목차)
  - [설치](#설치)
  - [Basic Properties](#basic-properties)
    - [Text](#text)
      - [Color](#color)
      - [font(size/line-height/letter-spacing/word-spacing)](#fontsizeline-heightletter-spacingword-spacing)
      - [font-family](#font-family)
      - [font-style](#font-style)
      - [font-weight](#font-weight)
      - [text-align](#text-align)
      - [etc](#etc)
      - [stroke](#stroke)
    - [Box](#box)
      - [Size](#size)
      - [Radius](#radius)
      - [Fill](#fill)
      - [Stroke](#stroke-1)
      - [Effects](#effects)
      - [Clip content](#clip-content)
      - [Scroll](#scroll)
    - [Layout](#layout)
      - [Flexbox](#flexbox)
      - [hbox](#hbox)
      - [vbox](#vbox)
      - [Autolayout (direaction, gap, padding)](#autolayout-direaction-gap-padding)
      - [Position](#position)
      - [Visibility](#visibility)

## 설치

- npm 설치

  > npm i -D adorable-css

- 설정 변경

  - vite.config.js

  ```js
  // vite.config.js
  import {adorableCSS} from "adorable-css/vite" // <-

  export default defineConfig({
  plugins: [adorableCSS(), ...] // <- plugin을 맨 처음에 등록합니다.
  })
  ```

- Import
  - main.js or main.tsx
  ```js
  // main.tsx or main.js 에 추가
  import "adorable-css";
  ```

## Basic Properties

### Text

#### Color

- c(red) c(#f00) c(#f00.5) c(255,0,0) c(255,0,0,.3) c(100%,0,0)

#### font(size/line-height/letter-spacing/word-spacing)

- font(20/1.4/-1%) font(20/1.4) font(20/-/-1%)
- font-size(30) line-height(1.5) -letter-spacing(-1px)
- word-spacing(-1px)

#### font-family

- sans-serif serif cursive monospace

#### font-style

- bold italic underline strike

#### font-weight

- 100 200 300 400 500 600 700 800 900
- thin light medium regular bold heavy

#### text-align

- text(left) text(center) text(right) text(justify)
- text(center+bottom) text(pack)

#### etc

- lowercase uppercase small-caps
- monospace(number)

#### stroke

- text-shadow()

---

### Box

#### Size

- w(30) w(~30) w(30~) w(20~30)
- h(30) h(~30) h(30~) h(20~30)

#### Radius

- r(10) r(fill) r(100%)

#### Fill

- bg(#B75959) bg(linear-gradient(#000,#fff)) bg(/image.png)

- cover contain

#### Stroke

- b(#000) b(1/#000) b(1/solid/#000)
- bt(#000) br() bb() bl()
- outline() ring()

#### Effects

- box-shadow(0/4/4/#000.25)
- box-shadow(inset/0/4/4/#000.25)
- blur(4)
- backdrop-blur(4)

#### Clip content

- clip overflow(hidden)
- nowrap... line-clamp(3)

#### Scroll

- scroll scroll-x scroll-y

---

### Layout

#### Flexbox

- hbox vbox pack

#### hbox

- hbox(top) hbox(top+center) hbox(top+right)
- hbox(left) pack hbox(right)
- hbox(bottom) hbox(bottom+center) hbox(bottom+right)
- hbox(fill)

#### vbox

- vbox(top) vbox(top+center) vbox(top+right)
- vbox(left) vbox pack vbox(right)
- vbox(bottom+left) vbox(bottom+center) vbox(bottom+right)

#### Autolayout (direaction, gap, padding)

- p(10) p(10/20) p(10/20/30) p(10/20/30/40)
- gap(10) gap(10/20)
- flex flex(1) flex(2)

#### Position

- static relative absolute fixed
- sticky sticky-top sticky-right sticky-bottom sticky-left
- layer() layer(top) layer(top+right)
- top() right() bottom() left() x() y() z()

#### Visibility

- none hidden visible blind opacity(.5)
