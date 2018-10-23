[_Matan Mazor_ suggested](https://medium.com/@mazormatan/cryptographic-preregistration-from-newton-to-fmri-df0968377bb2) to use a cryptographic hash as a seed to randomize stimulus sequences in order to prove that the preregistration happened before data collection. I thought this is a good idea. You can easily do it with JATOS. Here is how.

Each JATOS study has in its properties a field _Description_ where you can store a description of the study. Whatever one write in there is shown in the top of the study page. But behind the scene JATOS additionally calculates a hash of everything in _Description_ and stores it together with a timestamp in the study's [Study Log](http://www.jatos.org/Study-Log.html). This entry might look like this here:

```json
{
    "msg": "Study description changed",
    "studyDescriptionHash": "5ebcf2d8e30dcb721899fd1801be3c9f74de67b5d713ec00f50f94555b73ff69",
    "timestamp": "Tue, 23 Oct 2018 19:06:14 GMT"
}
```

Whenever you change the _Description_ JATOS recalculates the hash and stores a new entry in the Study Log. 

So now you already have a proof of the time when you wrote this _Description_ (if your JATOS server's time setup is correct).

What _Matan_ additionally suggests is to use this hash as an input to the calculations you might do in your experiment (e.g. as a seed for a random function). You can get the hash from the Study Log but there is also a `jatos.js` variable `jatos.studyProperties.descriptionHash` if you want to use it directly in your study's JavaScript.

Now you have a happens-before relationship between your _Description_ and your result data.
