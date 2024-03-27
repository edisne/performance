# Performance Optimization Tips in Angular

- lazy loading (defer loading if on angular 17, use schematics to convert form old control flow to 
  new `ng generate @angular/core:control-flow`)  - because it is importing modules at the runtime where it is resolved as a Promise
- use nx (prebuild caching for builds and tests), also it is possible to see the dependency graph
```
nx dep-graph --file=output.html

making production build with nx
nx run dashboard:build --prod
  
```

- change functions that structure data in HTML to pipes where possible, use @memo decorator for better caching of primitive values
- check for leaky subscriptions (unsubscribe)
- use webpack bundle analyzer (https://www.npmjs.com/package/webpack-bundle-analyzer) to check each module size and dependencies
```
In package.json

"build:stats": "ng build --stats-json"
"analyze": "webpack-bundle-analyzer dist/apps/appName/stats.json"
```

- Optimizing images: Angular Image Directive (75% improvement in LCP)
```
<img ngSrc="logo.png" witdth="200" height="150">
```

- Use OnPush change detection that only runs when:
```
- Input property reference changes
- Output property/event emitter or DOM event fires
- Async pipe receives an event
- Change detection is manually invoked via ChangeDetectorRef
```

- Add trackBy to every ngFor
- Tree shaking - dead code elimination - removing unnecessary modules from the bundle
```
One example could be:
importing *  as _ from loadash 

better instead of using default loadash package use loadash-es which exports each function as 
ES module and remove the unnecessary code from the bundle


Another example could be using the whole rxjs library:
import { Observable } from 'rxjs'

Better to use:
import { Observable } from 'rxjs/internal/Observable'

Also, take into consideration that it impacts css as well with using @import 
```
- Check if --optimization flag is used when building the project
- Use Chrome Profiler to detect "most expensive" operations/tasks
- Use Web Workers: For non-UI related tasks, consider offloading them to Web Workers.
- Implement hybrid rendering with Angular Universal
- Implement SSR
- Check if server compression is turned on
-  Use `defer` on scripts that are not required immediately
- For future-proof use CLI budgets to prevent regressions in the future

