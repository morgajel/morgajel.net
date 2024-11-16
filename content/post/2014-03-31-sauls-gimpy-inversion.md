---
author: Jesse Morgan
categories:
- Uncategorized
date: "2014-03-31T10:38:12Z"
guid: http://morgajel.net/?p=1484
id: 1484
title: Saul&#8217;s Gimpy Inversion
url: /2014/03/31/1484
---

Note for next time- If I ever need to invert the alpha and black on 40+ layer images, this script-fu will do the trick in gimp.

```
(define (get-all-real-layers image)
  (define (get-children group)
    (let loop ((children (vector->list (cadr (gimp-item-get-children group))))
               (sub-layers '()) )
      (if (null? children)
        (reverse sub-layers)
        (loop (cdr children)
              (if (zero? (car (gimp-item-is-group (car children))))
                (cons (car children) sub-layers)
                (append sub-layers (get-children (car children))) )))))
  (let loop ((top-layers (vector->list (cadr (gimp-image-get-layers image))))
             (all-layers '()) )
    (if (null? top-layers)
      all-layers
      (loop (cdr top-layers)
            (if (zero? (car (gimp-item-is-group (car top-layers))))
              (append all-layers (list (car top-layers)))
              (append all-layers (get-children (car top-layers)))) ))))

(map
  (lambda (layer)
    (gimp-image-select-item image CHANNEL-OP-REPLACE layer)
    (gimp-drawable-fill layer FOREGROUND-FILL)
    (gimp-edit-clear layer) )
  (get-all-real-layers image) )
```

Big thanks to saul on irc.gimp.net for this snippet.