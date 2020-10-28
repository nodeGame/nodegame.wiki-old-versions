- status: complete
- version: 5.x

It is strongly recommended that you always ask for the participants'
informed consent to participate in a study.

## Main Page

Below is the page template, in which you need to replace `<!-- Insert
Consent Form Here -->` with your own consent form.

```html
<!doctype html>
<link rel="stylesheet" href="/lib/bootstrap/bootstrap.min.css" />
<link rel='stylesheet' href='/stylesheets/nodegame.css'></link>
<style>
  #consent h3 { color: chocolate };
</style>
<body>
  <div id="container">
    <div id="notAgreed" style="display: none; margin-top: 30px; font-size: 24px;">
      <p>
        You have chosen to <strong>not participate</strong> in this study,
        therefore you have been disconnected and you can now return the HIT.
      </p>
      <p>
        Thank you for your interest, we hope to have you as a participant in our
        future studies.
      </p>
      <button class="btn" id="show-consent">Show Consent Form</button>
    </div>
    <div id="consent">


      <!-- Insert Consent Form Here -->


      <!-- Print Consent Form -->
      <p>
        <br/> <input class="btn" type="button" value="Print this page"
        onclick="window.print()" /> <br/> <br/>
      </p>

      <!-- Buttons to express consent -->
      <strong>Do you understand and consent to these terms?</strong><br/>

      <div style="margin-top: 20px;">
        <button class="btn-copy btn-info-copy" id="agree"> Yes, I Agree
        </button>

        <button class="btn-copy btn-info-copy" id="notAgree">
          No thanks, I do not want to do this HIT
        </button>
      </div>
    </div>
</body>
```

## Consent Forms

Below you can find two informed consent templates for university and
corporate research. Just replace the text in curly brackets with the
information which is relevant for you.

### University Research Consent Form Template

```html
<p>
  We would like to invite you to participate in a web-based
  online experiment. The experiment is part of a research
  program whose purpose is {EXP_PURPOSE}.
</p>

<p>
  <strong>You must be at least 18 years old to participate
    in these experiments.</strong>
</p>

<p>
  <strong>The decision to participate in this research
    project is voluntary.</strong> You do not have to
  participate and you can refuse to answer any
  question. Even if you begin the web-based experiment, you
  can stop at any time by closing your browser.
</p>

<p>
  <strong>There are no foreseeable risks or discomforts to you
    for taking part in this study.</strong> We anticipate that
  it will take about
  <em><span id="time"></span> minutes</em> to complete this
  task. However, you will only be rewarded for completing
  the task, not for the length of time you participate.
</p>

<p>
  You will be compensated for good faith participation in this
  experiment with <em>$<span id="money"></span> {CURRENCY}</em>.
  Submitted HITs will be <strong>rejected</strong>
  if they are completed in a manner that appears to be random,
  incomplete, or otherwise negligent.
</p>

<p>
  <strong>Your part in this study is anonymized to the
    researchers.</strong> Researchers at the {RESEARCH_LAB}
    may have access to anonymized experimental research
    records. However, because of the nature of electronic
    systems, it is possible that respondents could be
    identified by some electronic record associated with the
    response. Neither the researcher nor anyone involved with
    this study will be capturing those data. Any reports or
    publications based on this research will use only group
    data and will not identify you or any individual as being
    affiliated with this project. The only
    individually-identifying data we receive from Mechanical
    Turk are your unique identifier and your country.  Your
    data are also stored on Mechanical Turk's servers, and the
    data there are subject to the Amazon Mechanical Turk
    Privacy Notice and Participation Agreement.
</p>

<p>
  <strong>If you have any questions regarding electronic privacy,</strong>
  please feel free to contact {PRIVACY_CONTACT}.
</p>

<p>
  <strong>If you have any questions about this study,</strong>
  you may contact {EXPERIMENTER_NAME}.
</p>

<p>
  <strong>If you have any questions regarding your rights as a
    research participant,</strong> please contact
  {IRB_CONTACT}. You may call anonymously if you wish.
</p>

<p>
  <strong>This study has been reviewed and approved by the
    {IRB_APPROVAL}. This study is funded, in part, by
    {FUNDING_SCHEME}.</strong>
</p>

<p>
  <strong>By clicking
    below you are indicating that you consent to participate in this study. Please print out
    a copy of this consent form for your records.</strong>
</p>
```

### Corporate Research Consent Form Template

```html

This research project has been reviewed and approved by the {COMPANY}
Ethics Advisory Board.

<h3>INTRODUCTION</h3>

<p>
  Thank you for deciding to volunteer in a {COMPANY} project. The
  purpose of this project is {EXP_PURPOSE}.
</p>

<p>
  You have no obligation to participate and you may decide to terminate
your participation at any time. You also understand that the researcher
has the right to withdraw you from participation in the project at any
time. Below is a description of the research project, and your consent to
participate. Read this information carefully.
</p>

<h3>TITLE OF RESEARCH PROJECT</h3>

{EXP_TITLE}

<h3>PROCEDURES</h3>

<p>
  The expected duration of this task is
  about <em><span id="time"></span> minutes</em>, after completing the
  HIT you will be compensated
  with <em>$<span id="money"></span> {CURRENCY}</em>.
</p>

<p>
During this project, the following will happen: {EXP_DESCR}.
</p>

<h3>PERSONAL INFORMATION</h3>

<p>
This waiver is intended to give you informed consent regarding your
participation in this project and also to protect your personally
identifiable information by not asking for specific details, such as your
name.
</p>

<p>
By clicking "Yes, I agree" at the end of this form, you are agreeing that
you've had time to read and consider this consent waiver and are
comfortable with what is being asked of you as a participant. Aside from
your Mechanical Turk ID, no personal information will be collected during
this study. Your Mechanical Turk ID will not be shared outside of
{COMPANY} and the confines of this study without your permission,
and will be promptly deleted after compensation has been successfully
provided (30 days or less) and after the study has been
completed. De-identified data may be used for future research or given to
another investigator for future use without additional consent.
</p>

<p>
  If you wish to review or copy any personal information you provided
  during the study, or if you want us to delete or correct any such data,
  email your request to {EXPERIMENTER_NAME} at:
  {EXPERIMENTER_EMAIL}. We will respond to questions or
  concerns within {DAYS_TO_ANSWER} days. For additional information on how {COMPANY}
  handles your personal information, please see
  the <a href="{LINK_TO_PRIVACY_STATEMENT}"
  target="_blank">{COMPANY} Privacy Statement</a>.
</p>

<h3>RESEARCH RESULTS & FEEDBACK</h3>

{COMPANY} will own all of the research data and analysis and other results
(collectively "Research Results") generated from the information you
provide and your participation in the research project. You may also
provide suggestions, comments or other feedback ("Feedback") to {COMPANY}
with respect to the research project. Feedback is entirely voluntary, and
{COMPANY} shall be free to use, disclose, reproduce, license, or otherwise
distribute, and leverage the Feedback and Research Results.

<h3>CONFIDENTIALITY</h3>

The research project and information you learn by participating in the
project is confidential to {COMPANY}. Accordingly, you agree to keep it
secret as you would your own confidential information and never disclose
it to anyone else (unless you are required to do under judicial or other
governmental order). However, you do not need to keep secret specific
information that is general public knowledge or that you legally receive
from another source that is not affiliated with {COMPANY} so long as that
source was entitled to share the information with you and did not obligate
you to keep it a secret. You agree not to disclose to {COMPANY} any
non-public information, whether yours or a third party's without notifying
{COMPANY} in advance.


<h3>BENEFITS AND RISKS</h3>

<ul>
  <li><strong>Benefits:</strong> The research team expects to {EXP_BENEFITS}.
  You will receive any public benefit that may come these
  Research Results being shared with the greater scientific
  community.</li>

  <li><strong>Risks:</strong> During your participation, you may experience
    risk that should not be any more significant than the risks you
    experience in your regular daily routine.</li>
</ul>

<p>
You accept the risks described above and whatever consequences may come of
those risks, however unlikely, unless caused by our negligence or intentional misconduct.
</p>

<p>
You hereby release {COMPANY} and its affiliates from any claim you may
have now or in the future arising from such risks or consequences. In
addition, you agree that {COMPANY} will not be liable for any loss,
damages or injuries that may come of improper use of the study prototype,
equipment, facilities, or any other deviations from the instructions
provided by the research team.
</p>

<p>
Don't participate in this study if you feel you may not be able to safely
participate in any way including due to any physical or mental illness,
condition or limitation. You agree to immediately notify the research team
of any incident or issue or unanticipated risk or incident.
</p>

<h3>YOUR AUTHORITY TO PARTICIPATE</h3>

<p>
You represent that you have the full right and authority to sign this
form, and if you are a minor that you have the consent (as indicated
below) of your legal guardian to sign and acknowledge this form. By
clicking "Yes, I agree" below, you confirm that you understand the purpose
of the project and how it will be conducted and consent to participate on
the terms set forth above.
</p>

<p>
Should you have any questions concerning this project, please contact
{EXPERIMENTER_NAME} at {EXPERIMENTER_EMAIL}. Please confirm
your acceptance by clicking "Yes, I agree" below.
</p>

<p>
Upon request, a copy of this consent form will be provided to you for your
records. On behalf of {COMPANY}, we thank you for your contribution and
look forward to your research session.
</p>
```

### Explanations of Variables to Replace

Note that some variables template-specific.

- {EXP_TITLE}: a concise title for the study,
- {EXP_PURPOSE}: the purpose of your study (try to not give away
  too much, a generic text such as "to study how groups perform
  decisions" might suffice),
- {EXP_DESCR}: a more verbose description of the different stages of
  the experiment,
- {EXP_BENEFITS}: a verbose description of the benefits for society
  and science for conducting your study,
- {RESEARCH_LAB}: the name of your research group,
- {PRIVACY_CONTACT}: the person at your institution answering privacy
  concerns,
- {EXPERIMENTER_NAME}: this is you,
- {EXPERIMENTER_EMAIL}: the email you want participants to write to in
  case of issues with the study,
- {IRB_APPROVAL}: the person/body that approved your study, add the
  approval number if any,
- {FUNDING_SCHEME}: the funding declaration,
- {CURRENCY}: the currency in which you pay participants,
- {COMPANY}: the name of the company which commissioned the study,
- {DAYS_TO_ANSWER}: expected number of days to answer an email,
- {LINK_TO_PRIVACY_STATEMENT}: an hyperlink to an official privacy
  statement.

### Accept/Reject the Informed Consent

In the `player.js` you then simply add:

```js
stager.extendStep('consent', {
    frame: 'consent.htm',
    donebutton: false,
    cb: function() {
        var a, na;
        // You must define these variables in the settings.
        W.setInnerHTML('time', node.game.settings.time);
        W.setInnerHTML('money', node.game.settings.WIN);
        a = W.gid('agree');
        na = W.gid('notAgree');
        a.onclick = function() { node.done(); };
        na.onclick = function() {
            node.say('notAgreed');
            a.disabled = true;
            na.disabled = true;
            a.onclick = null;
            na.onclick = null;
            node.socket.disconnect();
            W.hide('consent');
            W.show('notAgreed');
            W.gid('show-consent').onclick = function() {
                var div, v;
                div = W.toggle('consent');
                v = div.style.display === '' ? 'Hide' : 'Show';
                v += ' Consent Form';
                this.innerHTML = v;
            };
        };
    }
});
```

## Go Back to

* [Home](Home)
