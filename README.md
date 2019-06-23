# React Native Best Practices
A slightly polemic repository with shared knowledge and best practices from React Native projects at Futurice.

[![Spice Program Sponsored](https://img.shields.io/badge/chilicorn-sponsored-brightgreen.svg?logo=data%3Aimage%2Fpng%3Bbase64%2CiVBORw0KGgoAAAANSUhEUgAAAA4AAAAPCAMAAADjyg5GAAABqlBMVEUAAAAzmTM3pEn%2FSTGhVSY4ZD43STdOXk5lSGAyhz41iz8xkz2HUCWFFhTFFRUzZDvbIB00Zzoyfj9zlHY0ZzmMfY0ydT0zjj92l3qjeR3dNSkoZp4ykEAzjT8ylUBlgj0yiT0ymECkwKjWqAyjuqcghpUykD%2BUQCKoQyAHb%2BgylkAyl0EynkEzmkA0mUA3mj86oUg7oUo8n0k%2FS%2Bw%2Fo0xBnE5BpU9Br0ZKo1ZLmFZOjEhesGljuzllqW50tH14aS14qm17mX9%2Bx4GAgUCEx02JySqOvpSXvI%2BYvp2orqmpzeGrQh%2Bsr6yssa2ttK6v0bKxMBy01bm4zLu5yry7yb29x77BzMPCxsLEzMXFxsXGx8fI3PLJ08vKysrKy8rL2s3MzczOH8LR0dHW19bX19fZ2dna2trc3Nzd3d3d3t3f39%2FgtZTg4ODi4uLj4%2BPlGxLl5eXm5ubnRzPn5%2Bfo6Ojp6enqfmzq6urr6%2Bvt7e3t7u3uDwvugwbu7u7v6Obv8fDz8%2FP09PT2igP29vb4%2BPj6y376%2Bu%2F7%2Bfv9%2Ff39%2Fv3%2BkAH%2FAwf%2FtwD%2F9wCyh1KfAAAAKXRSTlMABQ4VGykqLjVCTVNgdXuHj5Kaq62vt77ExNPX2%2Bju8vX6%2Bvr7%2FP7%2B%2FiiUMfUAAADTSURBVAjXBcFRTsIwHAfgX%2FtvOyjdYDUsRkFjTIwkPvjiOTyX9%2FAIJt7BF570BopEdHOOstHS%2BX0s439RGwnfuB5gSFOZAgDqjQOBivtGkCc7j%2B2e8XNzefWSu%2BsZUD1QfoTq0y6mZsUSvIkRoGYnHu6Yc63pDCjiSNE2kYLdCUAWVmK4zsxzO%2BQQFxNs5b479NHXopkbWX9U3PAwWAVSY%2FpZf1udQ7rfUpQ1CzurDPpwo16Ff2cMWjuFHX9qCV0Y0Ok4Jvh63IABUNnktl%2B6sgP%2BARIxSrT%2FMhLlAAAAAElFTkSuQmCC)](https://spiceprogram.org/)

## License

[Futurice Oy](http://www.futurice.com)
Creative Commons Attribution 4.0 International (CC BY 4.0)

## Summary

1. Starting a React Native project

* Using starter kits
  * Expo
  * CRNA
  * Vanilla

* Structuring files
  * Common components
  * Feature-based vs Screen-based

2. Developing

* Functional / Class Components
* Routing
* Styling
  * Emotion vs styled-components vs typestyle vs stylesheet

  Emotion 
* Integrating with Redux
* Form data management
  * Formik vs redux-form vs vanilla
* Animations
  * Static ( Lottie )
  * Between components

3. Distribution

* App Center
* Circle CI
* Manual

## Form data management

**In short,** avoid `redux-form`, begin with simple state, check out `formik` if needed

React Native in practice has no opinion regarding data management, the amount of possible approaches is numerous. However, due to mobile nature of RN applications a few requirements must be considered – an ability to easily split forms into steps or screens and persisting data across screens.

Numerous approaches are widely used in RN applications, the table below outlines most of them and gives an opinion based on previous experience with the technology

Name | Good parts | Bad parts | Opinion
--- | --- | --- | ---
[redux-form](https://github.com/erikras/redux-form/) | Might be easy to install if you already have Redux in place | Extra level of abstraction. Redux store gets polluted with unnecessary data. Making changes triggers the whole sequence of actions being dispatched. Big library size ([100.3 kB minified](https://bundlephobia.com/result?p=redux-form@8.2.4)) | In most of the projects maintenance was making developers life much harder, making managing data a painful experience. Adviced against use of the library
[formik](https://github.com/jaredpalmer/formik) | Changes are stored in internal state of a parentcomponent. Nice support for field validation and error handling. Nicer library size ([43 kB minified](https://bundlephobia.com/result?p=formik@1.5.7)) | Since data is stored in formik's component's internal state it might be hard to keep track of all changing events. | A performant and simple solution. Flexible API allows to handle input validation and most of the typical operations
`state` | Performance, "nativeness", any type of custom scenarios, flexibility, transparency | Creating logic for custom validation and error handling might be time consuming. Created custom patterns might be difficult to maintain in future | The best approach if no validation is required or the project is relatively small. Can be easily refactored to formik in case of increased complexity

`redux-form` is one of the most popular libraries for managing form data. However, judging from the experience of many developers, this approach was brining more problems rather than benefits. First of all, the idea of keeping form's state and all validation results and errors as well as touched/blurred/registered states fills the store with irrelevant data. Making changes to input fields launches redux action, which can make it hard to travers redux events history.
While `formik` is a great solution, it is based on basic state manipulation, so the advised approach would be to start with `state` actions whether using hooks or not and refactor to `formik` if needed.