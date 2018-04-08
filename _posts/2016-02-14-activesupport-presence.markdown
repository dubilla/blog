---
layout: post
title: "Active Support Presence and the Single Line Method"
date: 2016-02-14 15:00
comments: true
categories: web-development
published: true
---
Ruby and Rails are both built to help us write concise, meaningful code. One pattern I constantly find myself writing is a method that returns one thing, if it exists, or a default value. This can work well upfront, but it takes a little more care if the case gets any more complex. Fortunately, Rails has just the trick to keep the more complex case nice and lean in its ActiveSupport gem.

<!-- more -->

Let's look at an example that works without Rails.

``` ruby
def title
  label || 'New Page'
end
```

The above looks great. It will return label, if it exists, or it falls back to returning 'New Page'. I'm pretty thrilled with it. But what if we need to `titleize` the label?

``` ruby
def title
  label.titleize || 'New Page'
end
```

Unfortunately, the above will throw an error if `label` is nil. That kind of defeats the purpose of the conditional in the first place. At this point, it'd be easy to toss away our desire for concisement and write the following:

``` ruby
def title
  if label.present?
    label.titleize
  else
    'New Page'
  end
end
```

Our new method will work. But how can we leverage Rails to make our method a little sharper? ActiveSupport comes with a `#presence` method that returns the original object, if it exists, or `nil` otherwise. Let's see it in action.

``` ruby
def title
  label.titleize.presence || 'New Page'
end
```

Boom! Our method is nice, lean, and readable. `#presence` is the secret to manipulating and returning a variable before it exists or falling back to a default.
