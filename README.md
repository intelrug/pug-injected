# pug-injected

These mixins are for injecting child blocks in the parent mixin. Please, mind that this solution is temporary and I'm really looking forward to the Pug's 3.0.0 release with a support for Multiple Blocks in Mixins.

## Installation
Install pug-injected package to your project
```cmd
npm i -D intelrug/pug-injected
```
Include pug file to your project
```jade
include NODE_MODULES_PATH/pug-injected/pug-injected
```

## Usage
Mixin
```jade
mixin parent()
    +injected: block
    .parent
        +if('top')
            .parent__top
                +use('top')
        +if('box')
            .parent__boxes
                +each('box')
                    .parent__box
                        +use('box')
        +else('box')
            .parent__empty
                | Box is not inject

```
Usage
```jade
+parent()
    +inject('top')
        | Text for top position
    +inject('box')
        | Text for first box
    +inject('box')
        | Text for second box
```
Result
```html
<div class="parent">
    <div class="position-top">Text for top position</div>
    <div class="position-boxes">
        <div class="position-box">Text for first box</div>
        <div class="position-box">Text for second box</div>
    </div>
</div>
```

## Warnings

Since it's a temporary fix, there are some limitations.

### Don't use a `block` if `+injected` called in mixin

```jade
mixin parent()
    +injected: block
```

### Don't use data cycles (hopefully will be fixed soon)
```json
this expamle has error
```
```jade
+parent()
    +inject('top')
        | Text for top position
    each item in boxes
        +inject('box')
            =item
```


## Mixins

### Initializing the inject into mixin
```jade
+injected: block
```

### Use block
Insert the block. Mind that if you call use mixin, you need to check if there is a block.
```jade
+use(blockName)
```

### If-Else
```jade
+if(blockName)
    +use(blockName)
+else(blockName)
    =blockName + ' is not injected'
```

### Each
Each injected blocks
```jade
+each('box')
    +use('box')
```
### Inject
Injected blocks into parent mixin
```jade
+parent
    +inject('box')
        | content
```
