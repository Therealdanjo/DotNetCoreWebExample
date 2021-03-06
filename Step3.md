# Step 3 - Add web site

## Server Configration

* Add a new folder called wwwroot

### Activate Static Files in the startup

<details><summary>Configure</summary>

~~~c#
// activate static files serving
app.UseDefaultFiles();
app.UseStaticFiles();
~~~
</details>

## Website

* Add an index.html file to wwwroot

<details><summary>Add Twitter Bootstrap, Vue.JS and axios scripts imports</summary>

~~~Html
<head>
<!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios@0.18.0/dist/axios.min.js" crossorigin="anonymous"></script>
    <title>Ask your questions</title>
</head>
<body>
    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>
</body>
~~~
</details>

<details><summary>Create the layout</summary>

~~~Html
<nav class="navbar navbar-dark bg-dark">
    <a class="navbar-brand" href="#">Questions</a>
    <button class="btn my-2 my-sm-0" data-toggle="collapse" data-target="#questionBox">Ask</button>
</nav>

<div id="questionView">
    <div class="collapse" id="questionBox">
        <div class="container mt-4">
            <div class="card card-body">
                <div class="row">
                    <div class="col-11">
                        <input type="text" class="form-control" v-model="newQuestion" placeholder="Question">
                    </div>
                    <div class="col-1">
                        <button class="btn btn-primary btn-block" v-on:click="add"
                                data-toggle="collapse" data-target="#questionBox">Send</button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="container mt-4">
        <table class="table">
            <thead>
                <tr>
                    <th scope="col" class="col-8">Question</th>
                    <th scope="col" class="col-3">Votes</th>
                    <th scope="col" class="col-1"></th>
                </tr>
            </thead>
            <tbody>
                <!-- display a tablerow for every item in the questionList -->
                <tr v-for="item in questionList">
                    <td>{{ item.content }}</td>
                    <td>{{ item.votes }}</td>
                    <td>
                      <button class="btn" style="font-size: xx-small" v-on:click="vote(item)">&#x25B2;</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</div>
~~~
</details>
  
<details><summary>Add the vue.js controller to make the site dynamic</summary>

~~~Html
<script>
    // Controller for the page
    var questionListController = new Vue({
        el: '#questionView',
        data: {
            // the list of questions, displayed in the table
            questionList: [],
            // the value of the "add new question" edit
            newQuestion: '',
        },
        methods: {
            // adds a new question to the list
            add: function (event) {
                // if the question is empty, call stopPropagation to stop twitter bootstrap from colapsing the card
                if (this.newQuestion == '') {
                    event.stopPropagation();
                    return;
                }
                // call the api to add a new question
                axios
                    .put('api/Commands/Questions/ask?' + $.param({ content: this.newQuestion }))
                    .then(() => {
                        this.getQuestions();
                        this.newQuestion = '';
                    })
                    .catch(function (error) { alert(error.response.data); });
            },
            vote: function (question) {
                // call the api to increment the votes of the question
                axios
                    .put('api/Commands/Questions/vote?' + $.param({ questionId: question.id }))
                    .then(() => this.getQuestions())
                    .catch(function (error) { alert(error.response.data); });
            },
            getQuestions: function () {
                axios
                    .get('api/Queries/Questions')
                    .then(response => (this.questionList = response.data))
            }
        },
        mounted: function () {
            this.getQuestions();
        }
    });
</script>
~~~
</details>