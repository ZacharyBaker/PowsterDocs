# PowsterDocs

## Quiz Component

#### What Are You?

Component which renders the quiz title and start button or renders the `Questions` component -- contains the logic to either start the quiz or if the quiz has been started, render the Questions

#### Who Else Is Involved In Your Life?

* Application State -- getting passed the state of the app
* Styles -- quiz.sss
* Components -- pulling in the Questions component
* Utils -- for tracking
* node_modules -- preact

#### What Is Rendered

Depending on the `state.questionMode` it renders the title, subtitle (if it exists), and a start button for a given quiz
If the quiz has been started it will render the `Questions` component

#### State Handling

Manages its own state by initializing `state.questionMode` to `false` and changing the value by invoking the `startQuiz` function triggered by the click event on the button, at which point the `Questions` component is rendered
Takes in `{appData, pageData, transitionState}` and passes them as props to the `Questions` component

#### Functions

`startQuiz` -- Sets the `questionMode` property on the `state` object to `true`, enabling the questions to be rendered




## Questions Component

#### What Are You?

In this instance it is the child component to Quiz -- renders a question and its answers, then renders the next 
question after an answer is clicked, once all the questions have been answered, 
it reroutes to a page where the results are displayed

#### Who Else Is Involved In Your Life?

* Application State -- getting passed the state of the app
* Styles -- quiz.sss
* Components -- importing the `Transition` and `SimpleVideoPlayer` components
* Utils -- for tracking and determining currentPage and relativeRoot
* node_modules -- preact
* Video assets

#### What Is Rendered

It renders a question and its answers.  It does this by mapping over the answers for the current
question in a random order and for each answer rendering a video player (which autoplays for mobile devices), answerId, title and subtitle if it exists
It also dynamically renders the correct video for each answer by setting the `src` attribute 
equal to the result of invoking the `getAssetName` method with the `activeQuestion` and `answer` parameters

#### State Handling

Initially it sets `state.activeQuestion` to `0`, `state.results` to an empty object and determines what device is being used -- the state is changed/set through a click event on the answers
of the `activeQuestion` invoking the `nextQuestion` method which contains the logic to set the state
of the component

#### Functions

`nextQuestion` --  It takes the `type` param, so it knows which property on the state.results object to update
If it determines that it is not the last question it accumulates the results in the state 
object and sets the state.activeQuestion to nextQuestion, essentially incrementing the value by 1
which causes the component to rerender with the updated information
If the current question is the final question, it sets the state with the final answer and routes to another location to display the results
Invokes the tracking function three times to update the client on the progress of the quiz and if it is finished, invokes the tracking function once more with the `'quiz-finished'` action

`showResults` -- Captures the result of the quiz by calling `Array.reduce` on an array of 
properties from the results object 
Invokes tracking function with the `'quiz-results'` action
Determines the href and reroutes to the results page

`getAssetName` -- By passing in the `questionNum` and `answerId`, dynamically sets the `assetName`,
then checks if the device is mobile which sets a new value to `assetName` which is then used in the `src` attribute of the video player to grab the correct video

`playOnHover` -- `pauseOnHoverOut` -- Targets the elements by the ref attribute dynamically set
on the `SimpleVideoPlayer` component in order to play the video while hovering over the element 
and pause the video when no longer hovering

### How Are You Styled?

Reaching into the `quiz.sss` file and setting the `class` attribute on the element with the styles associated with the given variable

I have never seen styles working this way before.  Passing in the styles from the `quiz.sss` file to a `style` attribute makes more sense to me

### How To Optimise?

###### remove modules that aren't used

**Quiz Component**

```javascript
import Transition from 'components/core/transition/transition.js';
import Store from 'store/store';
```

###### use variables already declared

**Questions Component**

```javascript
let eachAnswer = data.questions[activeQuestion].answers[answer];
```
change to
```javascript
let eachAnswer = eachQuestion.answers[answer]
```

###### Preferencial Maybes

There are many instances when using `let` for a variable declaration when it is not being used, consider `const`
Using `PropTypes` with `preact-compat` or implementing them manually
Breaking down the `Questions` component into an `Answer` component and rendering the `Answer` component while mapping over the answers, passing in the necessary data as props





