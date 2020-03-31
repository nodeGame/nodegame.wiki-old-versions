- status: complete
- version: 3.x

[Amazon Mechanical Turk (AMT)](http://mturk.com) is a major online
platforms for recruiting experimental participants. nodeGame currently
offers partial integration with AMT via the [nodeGame-mturk
package](https://github.com/nodeGame/nodegame-mturk).


## Preparation

The experimenter needs a valid AMT
[requester account](https://requester.mturk.com/). The HIT creation
can be carried out using the AMT interface.


1. Before creating a new project, create a new _Qualification_. You
will use this to identify who participated in your experiment. Go to
Manage/Qualification Types/Create New Qualification Type.

2. Create a new project. Go to Create/New Project and choose the
template called _Survey Link_.

3. Choose Project Name (for own reference), Title (what will be
displayed to participants), Description, and Keywords.

4. Add all other settings as required by your experimental
design. Keep in mind that AMT guarantees that one worker can do at
most one assignment, as long as you have only one HIT in your group
(exactly what you want, and what you achieve following this tutorial).

5. You are likely going to run multiple sessions of the same
experiment, therefore you should avoid that workers that participated
in previous sessions join future ones. To achieve this, in the _Worker
Requirements_ select the Qualification that you have created at step
1. and choose _has not been granted_ as option. You have now to
remember to grant this qualification to your participants after each
experimental session is completed.

6. Proceed to the _Design Layout_. Modify the existing template by
clicking on _Source_ and replacing the entire text with the content of
the file _mturklinkpage.html_ that you find in the public folder of
the
[nodeGame-mturk package](https://github.com/nodeGame/nodegame-mturk). 

7. Make sure to adapt the content _mturklinkpage.html_ for your
purpose. In particular:
   - Replace all text enclosed in curly brackets {} with the
   appropriate content relevant for your experiment.
   - In the enclosed JavaScript script replace `X` and `Y` variables with
     the minimum resolution you would like participants to have.
   - In the encolsed JavaScript script replace the variable `host`
     with the url of your nodeGame server.

8. Click _Finish_.

9. It is recommended to also create another project called _Trouble
Ticket_ with 0 dollars reward for submitting a HIT. In this project
participant can report problems with your experimental HIT, and you
will be able to assign them a bonus, if needed. In fact,
there is no other way to pay them if, for example, they fail to submit
their HIT in time for any reason.


## Running the HIT

Make sure that your nodeGame server is up and running and properly
configured. That is, your `auth.settings.js` should have authorization
enabled and contain the following settings:

```javascript
// Remote clients will be able to claim an id via GET request
// from the task description page.
claimId: true,

// Validates incoming requests.
claimIdValidateRequest: function(query, headers) {
    if ('string' !== typeof query.a || query.a === '') {
        return 'missing or invalid AssignmentId';
    }
    if ('string' !== typeof query.h || query.h === '') {
        return 'missing or invalid HITId';
    }
    return true;
},

// Stores information about worker in the database.
claimIdPostProcess: function(code, query, headers) {
    code.WorkerId = query.id;
    code.AssignmentId = query.a;
    code.HITId = query.h;
}
```

## After the HIT is completed

Make sure you are saving a _results file_ at the end of your
experimental session. This results file must be saved manually in
comma separated value format (".csv") and should contain the following
header:

```
"access","exit","WorkerId","hid","AssignmentId","bonus","Approve","Reject"
```

Every line should contain the earned bonus, and an "x" either under
"Approve" or under "Reject".

Follow the instructions in the README.md of the
[nodeGame-mturk package](https://github.com/nodeGame/nodegame-mturk)
to:

- approve/reject each assignment,
- grant a bonus to each assignment,
- assign the qualification

Important! Make sure to assign the qualification also to dropouts.

## Next

* [Other Readings on AMT](https://github.com/nodeGame/nodegame/wiki/AMT)

## Source

* [nodeGame-mturk package](https://github.com/nodeGame/nodegame-mturk)

## Go Back to 

* [Home](Home)
