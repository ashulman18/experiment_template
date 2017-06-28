Inside the _shared are some helper js files, some libraries we call. Some of these are completely external, like jquery and raphael, some of them are written by members of the lab, like mmturkey and utils.

Inside your javascript file for your own experiment, e.g. template.js, you'll write up the structure that the slides have. In template.js, the line

    exp.structure=["i0", "instructions", "familiarization", "one_slider", "multi_slider", 'subj_info', 'thanks'];

defines the logical flow of the experiment. Always start with i0 (the consent slide) and move on from there. For easy editing, you can move the id of the slide you are editing forward in the flow so you do not have to step through.  The functions that use this structure to navigate through the experiment are in stream-V2.js and exp-V2.js.  Each slide has several parts:

1. the code in the `start` function is run at the beginning of that block. This is the similar to present_handle in other slides - you will read about this later.
2. if there is a `present` data structure (which can be a list of objects of any kind), each of these will be passed in, one at a time, to `present_handle`, for each trial in that block. 
3. `present_handle` has the parameter `stim` from the `present` data structure, and this code is run at the beginning of every trial. Every trial will have its own slide.
4. the code in `button` is called directly from the html where the button is defined. `_s` always refers to the current slide, so you can reference any function you define as a property of the slide by using the `_s.FUNCTION_NAME()` syntax. `_stream.apply(this);` is called to move to the next trial until there are no more objects to iterate through in the present array. 
5. the `exp.go()` function is run at the end of the block to proceed to the next slide.
6. you can define any other functions you want inside the slide and call them either with `this.FUNCTION_NAME` or `_s.FUNCTION_NAME()`.

We've provided a progress bar, which required that you calculate the total number of questions in your experiment (just run `exp.nQs = utils.get_exp_length();` inside the `init` function, at some point after `exp.structure` is defined). Turkers like this a lot.

To add a slide:   
- HTML: make a div class "slide" with a new page id in .html
- JS: make a slides.new_page = slide({...}) function (see rest of these functions for reference) with proper id
  - this should include a start/present_handle and a button 
- JS: add the new id to "exp.structure"