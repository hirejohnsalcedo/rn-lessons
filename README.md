# README

## Guard against bad svg
```
<View pointerEvents="none">
  <SvgXml xml={xml} />
</View>
```

## Nullish Coalescing Operator usage
Do not use the nullish coalescing operator `??`, when the falsy left-side expression should not be evaluated to `0` or `""`. (tags: avoiding unintended outcomes )

Even if the left-side expression is guaranteed to never be `0` or `""`

Given `someProp={myData ?? defaultValue}`

If `someProp` expects `typeof defaultValue`

There's no reason to prefer `??` instead of `||`

## Separation of concerns
Do not pass down implementation details from containers to screens or subcomponents (e.g. `componentId` ) if it can be defined at the container level. This improves readability and exposes intentionality when passing down props from the container. (tags: readability, segregation of responsibility)

For example, compare the two styles:

### Passing named function:
```
const MyContainer = (props) => {
  const handleNavigate = () => {
    Navigation.push(props.componentId, {...} )
  }
  return <MyScreen onNavigate={handleNavigate} />
}
const MyScreen = (props) => {
  return <Button onPress={props.onNavigate} />
}
```

### Passing implementation detail:
```
const MyContainer = (props) => {
  return <MyScreen componentId={props.componentId} />
}
const MyScreen = (props) => {
  const handleNavigate = () => {
    Navigation.push(props.componentId, {...} )
  }
  return <Button onPress={handleNavigate} />
}
```

In the second example, it takes a reviewer to go up to the `MyScreen` component before understanding why the `componentId` prop is being passed. Whereas in the first example, it's made more clear that the container just wants to give the subcomponents a way to navigate. Presentational components don't need to know about other components, states, or navigation. They're only concerned with receiving some data, and being able to display that data in a nice way.

## Factor out components
Don't let components be more than 3 indents deep. Factor them out so they're named and easier to read.

## Cross-platform TextInput custom font family
```
fontFamily: MY_FONT_FAMILY,
fontWeight: Platform.select({ ios: null, android: "100" }), // "100" should be any weight not equal to fontFamily's native weight
```

## Are you reviewing regarding style?
- Yes. Is it something fixable automatically by static code checking tools like prettier/lint?
  - Yes. Ok, let's add a rule so it gets automatically fixed
  - No. If it bothers you that much, change it yourself

## Pass implicitly typed static props
```
export const MyComponent = Object.assign(_MyComponent, { MY_STATIC_PROP });
```
