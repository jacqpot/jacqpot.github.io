---
layout: post
title:      "Join table Madness"
date:       2020-09-22 19:56:27 +0000
permalink:  join_table_madness
---


The Transition from a full rails app to a Javascript front end was smooth... until I tried to institute a many to many Join table. oh my lord. The mental hurdle was an olimpic class event! So I thought id break down the process. Here we go:

**a quick side note**
Naming conventions are **extreemly** important! 

Here are a few of the tables I Ended up with, Its important to get the exercises_workouts table just right. originaly I tried to create a has_and_belongs_to direct relationship, avoiding the join table alltogether. but I was not able to figure out the ajax post request using that method.
```
  create_table "exercises", force: :cascade do |t|
    t.string "bodyPart"
    t.string "fatigueRating"
    t.string "name"
    t.string "description"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  create_table "exercises_workouts", id: false, force: :cascade do |t|
    t.integer "workout_id", null: false
    t.integer "exercise_id", null: false
    t.index ["exercise_id", "workout_id"], name: "index_exercises_workouts_on_exercise_id_and_workout_id"
    t.index ["workout_id", "exercise_id"], name: "index_exercises_workouts_on_workout_id_and_exercise_id"
  end


  create_table "workouts", force: :cascade do |t|
    t.integer "volume"
    t.string "description"
    t.string "warmUp"
    t.date "date"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end
```

This rails backend join table sets us up for success. In this relationship, I created I finite amount of exercises using some seed data. This many to many relationship ensures that every workout could have the same instance of an exercise. 

I had to follow this up with a controler and model for the join table as a consiquence of steering away from the has_and_belongs_to relationships. 

```
class ExercisesWorkoutsController < ApplicationController


  def create
    @exercise_workout = ExercisesWorkout.new(exercises_workout_params)

    if @exercise_workout.save
      render json: @exercise_workout, status: :created
    else
      render json: @exercise_workout.errors, status: :unprocessable_entity
    end
  end

  private
    def exercises_workout_params
      params.require(:exercises_workouts).permit(:exercise_id, :workout_id)
    end
end


class ExercisesWorkout < ApplicationRecord
    belongs_to :workout
    belongs_to :exercise
end


```
as you can see, there is only one controler action. this prevents the user from removing exercises from a workout. That may change later, but for the project, this works great. 

With that said the back end is complete. Do note the singular and plural veriations of ExercisesWorkout and exercise_workout.



```
function exerciseFormSubmission(id){
    let form = document.createElement('div');
    let pick = document.getElementById("exercise").value;
    let workoutId = document.getElementById("workout_id").value;

    let workoutExercises = {
        exercises_workouts: {
            exercise_id: pick,
            workout_id: workoutId
        
        }
    }
		```
		this exerciseFormSubmission function was rather tough to figure out. for the first few lines I am grabing the elements from the DOM and setting them to variables.
		
		Then I am creating strong perams in preperation for the ajax call. note that the order oof attributes matters and needs to match the backend. Ive not quite figured out why.
		```
// debugger;
    fetch(baseUrl + `/exercises_workouts`,{
        method: "post",
        headers: {
            'Accept': 'application/json',
            'Content-type': 'application/json'
        },
        body: JSON.stringify(workoutExercises)
    })
    .then(resp => resp.json())
    .then(program => getWorkoutDetails(id))
    
    form.innerHTML = ""
}
```

for this post call I had to use the create method I showed you earlier in the ExerciseWorkout controller. Notice here that exercises and workouts is plural. So confusing! Finaly at the end you can see that I am taking the form that we got the params from is now set to an empty string. Beautifully hiding the form.  

All in all the transition from Rails to Js isnt so bad. Naming conventions are more and more important, building your own api is alot of fun, and finaly, Pay attention to the little stuff. the plurality of these names cost me hours of debugging. Good luck on your own projects and keep your head up!
