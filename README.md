# retest

Retest is a robust library that provides a suite of functions specifically designed for testing applications built with Reagent and Re-frame in ClojureScript. 

Key features include `click`, `fill`, and `datafy` which help emulate and validate diverse user interactions within your components. This powerful toolkit streamlines the testing process, thereby enhancing the overall reliability and robustness of your applications.

## Features

### click
The click function in the Retest library simulates a user action of clicking on an element, and notably, it supports event bubbling. 
```clj
(defn view
  []
  [:form
   [:button {:id ::foo :on-click (fn [_] (prn "clicked"))}]])

(retest.core/click [view] ::foo)

;; => "clicked"
```

### fill 
The fill function allows you to simulate filling in input fields. Here's an example of its usage:
```clj
(defn view
  []
  [:form
   [:input {:id ::foo
            :on-change (fn [event]
                         (prn #?(:clj  (-> @event :target :value)
                                 :cljs (.. event -target -value))))}]])

(retest.core/fill [view] ::foo "value")

;; => "value"
```

### datafy 
The datafy function allows you to extract the data from your components. Here's how you can use it:
```clj
(defn view 
  []
  [:div
    [:div {:itemProp :a} 1]
    [:div {:itemProp :b} 2]])

(sut/datafy [view])
    
;; => 
;; {:a "1"
;;  :b "2"}
```

```clj

(defn view 
  []
  [:div {:itemScope :c}
    [:div {:itemProp :a} 1]
    [:div {:itemProp :b} 1]
    [:div {:itemScope :d}
     (for [i [1 2 3]] ^{:key i}
       [:div {:itemProp :e} i])]])

(retest.core/datafy [view])

;; =>
;; {:c {:a "1"
;;     :b "1"
;;     :d [{:e "1"}
;;         {:e "2"}
;;         {:e "3"}]}}
```
