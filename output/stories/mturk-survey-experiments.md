<!-- 
.. link: 
.. description: A guide for running survey experiments on MTurk, like I did in my thesis
.. tags: science, mturk
.. date: 2014/02/11 16:11:44
.. title: MTurk Survey Experiments
.. slug: mturk-survey-experiments
-->

This is a work in progress for helping my fellow researchers run survey
experiments while avoiding some pitfalls.

# Template for HTML to use for your HIT on Amazon

I got the kernel for this from another social scientists blog, and I'd like to
attribute it. I just haven't had time to track it down!

```html
<!-- <p id='workerId'>No javascript run</p> -->
<style type="text/css"><!--
.highlight-box {
  border:solid 0px #98BE10;
  background:#FCF9CE;
  color:#222222;
  padding:4px;
  text-align:left;
  font-size: smaller;
}
#beforeAccept { display:block }
#workerNotFound { display:none}
#workerFound { display:none }
[type="submit"] { visibility:hidden }
--></style>

<!-- Google CDN is fast, but to avoid errors, we need to switch to amazon hosted -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"
    type="text/javascript"></script>
<!-- This is the recommended approach from crypto-js website -->
<script src="https://crypto-js.googlecode.com/svn/tags/3.1.2/build/rollups/sha1.js"
    type="text/javascript"></script>

<script type="text/javascript"><!--
function getParamFromURL(name) {
    // This was in the original code, not necessary
    // name = name.replace(/[\[]/,"\\[").replace(/[\]]/,"\\]");

    // MTurk screws up and (at least sometimes) puts & instead of & in the URL!
    // Regexps are char based, so we need only check for a ; prior to our
    // name. If we want to test for the whole & token, we need to work
    // outside of []'s and use |
    var regexS = "[\?&;]" + name + "=([^&#]*)";
    var regex = new RegExp(regexS);
    var results = regex.exec( window.location.href );
    if( results === null )
        return "";
    else
        return results[1];
}

// This is hashed to protect workers privacy (though the risk here is low)
// I don't care much about my privacy on the first line, compared to
// debugging, though!
// When done right, these should look like '942cf38dce8bcd7310476ae53fe5a906170c7d2d'

var hashed_workers = [CryptoJS.SHA1("A2DLTP7A10KG4T").toString(CryptoJS.enc.Hex),
                     ];

$(window).load(function() {
    // We now have an initial state where visibility is set in CSS
    var workerId = getParamFromURL('workerId');

    // This is for debugging, along with the commented span at the top
    // $("#workerId").text('Javascript detected worker ID "' + workerId + '"')

    if (workerId != "") {
        // Can't use inArray on full CryptoJS hash objects
        var hashedId = CryptoJS.SHA1(workerId).toString(CryptoJS.enc.Hex);

        // Is the (hashed) ID in our list of (hashed) IDs?
        if (jQuery.inArray(hashedId, hashed_workers) > -1){
            // Worker was found
            $("#beforeAccept").css('display','none');
            $("#workerFound").css('display','block');
        }
        else {
            // Worker was not found
            var link_elt = $('#surveyLink');

            link_elt.prop('href', link_elt.prop('href') + '&workerId=' + workerId);
            $("#workerNotFound").css('display','block');
            $("#beforeAccept").css('display','none');
            $("[type='submit']").css('visibility','visible');
        }
    }
});
--></script>

<div id="beforeAccept">
 <h3>Complete a short survey</h3>

 <div class="highlight-box">
  <p>We are conducting an educational research survey about political
  attitudes and scientific knowledge. You are allowed 60 minutes, but it
  will likely take you less than 30. You&#39;ll also probably learn
  something!</p>

  <p>If you accept this HIT, some javascript will run to see if you&#39;re
  elegible. If you&#39;ve taken another one of our surveys, you are
  ineligible for this one.</p>
 </div>
</div>

<div id="workerNotFound">
  <h3>Complete a short survey</h3>

  <div class="highlight-box">
    <p>Follow the link below to complete the survey. At the end of the
    survey, you will receive a code to paste into the box below to receive
    credit for taking our survey. Please give the survey your full attention
    (no web or e-mail).</p>
  </div>
  <!-- We have our base link here in case there's a snafu with updating the link -->

  <p><a href="http://ucbpsych.qualtrics.com/SE/?SID=SV_0AFVoIpzwIbnDqR" id="surveyLink" target="_blank">Survey link</a></p>

  <p>Provide the survey code here (or leave me a note if there&#39;s any problem):</p>

  <p><input id="Code" name="Code" size="10" type="text" /></p>
</div>

<div id="workerFound">
  <h3>Sorry</h3>

  <div class="highlight-box">
    <p>You&#39;ve done work on another one of our surveys. So, thanks for
    your interest, but please release the HIT!</p>
  </div>
</div>
```
