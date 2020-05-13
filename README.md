# Seperate-componentDidUpdate-from-componentDidMount-in-React-Hooks

useEffect is a very useful hook. It receives a callback function that executes when the component has mounted and every time it updates. So it works similarly to the old componentDidMount() and componentDidUpdate() methods for class components.

There is a tiny problem, though. Sometimes, we might not want it to work like both componentDidMount() and componentDidUpdate(). Sometimes we want to execute the hook only when the component mounts or only when it updates.

## How to keep useEffect from executing on mount

React doesn't really offer a clean way to do it. In future versions, it may be handled by an argument. But right now, the best way I've found is a reference hook.

## What is a reference hook?

We can use the reference to check whether the component has just mounted or updated. We initialise it as false and change the state to true on the first render. Then, we use this information to decide whether our useEffect should do something or not.

```
const App = props => {
  const didMountRef = useRef(false)
  useEffect(() => {
    if (didMountRef.current) {
      doStuff()
    } else didMountRef.current = true
  }
}
```

**This hook will check if didMountRef.current is true. If it is, it means that the component has just updated and therefore the hook needs to be executed. If it's false, then it means that the component has just mounted, so it will skip whatever action it is supposed to perform and set the value of didMountRef.current to true so as to know that future re-renderings are triggered by updates.**

Note! it is important to remember that what gets initialised when we define the reference hook is the refHook.current property, not the refHook itself.
