Friday, December 19, 2025: ---
In Cmder:
  343  cd E:\usersdonal\Code\javascript\svelte\Development\svelte-sound1
  344  git status
  345  npm init -y
  346  npm install --save-dev svelte rollup rollup-plugin-svelte rollup-plugin-terser @rollup/plugin-node-resolve @rollup/plugin-commonjs
  347  cd ..
  348  cd E:\usersdonal\Code\javascript\svelte\Development
  349  npx degit sveltejs/template svelte-sound1
  350  dir
  351  cd svelte-sound1
  352  npm install
  353  npm install --save-dev rollup-plugin-terser
  354  npm install --save-dev @rollup/plugin-terser
  355  npm install --save-dev rollup-plugin-css-only
  356  npm run dev
…then in another cmd window run: 
       npm run start
…and this works


Vokaler:
https://www.youtube.com/watch?v=wp96rLcxtC0		// excellent
https://www.youtube.com/watch?v=hzYArZVTD4s		// maybe
https://www.youtube.com/watch?v=U82ospRgS08		// okay

Tuesday, January 6, 2025: ---
Try Claude Code on this E:\usersdonal\Code\javascript\svelte\Development\svelte-sound1

Added folder E:\usersdonal\Code\javascript\svelte\Development\svelte-sound1\public\vokaler_ludvig, which has nine .opus files with names, without suffix, are: a, e, i, o, u, y, å, ä, ö
a.opus, e.opus, i.opus, o.opus, u.opus, y.opus, å.opus, ä.opus, ö.opus

Next: use CC to switch to using Ludvig's vokaler:
Modify the code so it uses the 9 sound-files in svelte-sound1\public\vokaler_ludvig instead of the mySounds folder. The vokaler_ludvig files map to the first 9 buttons in order: a.opus, e.opus, i.opus, o.opus, u.opus, y.opus, å.opus, ä.opus, ö.opus. These 9 buttons will be labeled in order: a, e, i, o, u, y, å, a, ö. The last button, tenth button, will play all 9 sound-files with 0.5 seconds pause between each. The last button will be labeled "All".
// works (but I had to use different browser to not get the old SPA)

Tell CC:
Make change: The "All" button plays each of the 9 files (a.opus, e.opus, i.opus, o.opus, u.opus, y.opus, å.opus, ä.opus, ö.opus) in sequence, with a pause of 0.5 seconds between playing each file.
// works

Make change: Clicking any button will first stop any currently playing sound-file from this SPA, then perform the action of that clicked button.
// works. Checked in.

Make change: Change the displayed label "Svelte Sound SPA" with the name of the sound-file folder used, currently "vokaler_ludvig", with the first letter of each sub-word (separated by "_") as uppercase, so will be "Vokaler_Ludvig".
Change the background color to the yellow-gold seen in the flag of Sweden. Change the button background to light-blue. Make the buttons twice as large and separate them by half their width.

// checked folder changes in.
// Use: npm run build
// Use: npm run start

Note that I can use a cmd window and 
cd E:\usersdonal\Code\javascript\svelte\Development\svelte-sound1
npm run build   // ignore errors
npm run start	// then open browser at listed URL, like http://localhost:53530


Friday, January 9, 2026: ----------------------------
Tell CC to put buttons in two rows, 

Change to use two rows of buttons, five on each. The buttons squeeze together if the browser window gets smaller.
// check in changes.

Add text under each button, using the same color and font used for the button labels but reduced in size to about 70%. 
Each button has two words of fewer than 10 characters length which are shown as two rows under each button. 
The upper row ten words are: 
"long1" "long2" "long3" "long4" "long5" "long6" "long7" "long8" "long9" " ".
The lower row ten words are: 
"short1" "short2" "short3" "short4" "short5" "short6" "short7" "short8" "short9" " ".
Note the last button has two blank words.

Saturday, January 10, 2026: 
Switch words to:
  const longLabels = ['apa', 'elbil', 'ishockey', 'odla', 'utan', 'ycklig', 'åsikt', 'äventyr', 'öde', ' '];
  const shortLabels = ['alla', 'eld', 'ilska', 'oktober', 'ursäkta', 'yrke', 'ånga', 'ängel', 'önska', ' '];
  
Check-in: Add the swedish example words. 

CC: Add a password which user must enter the first time they access the SPA. The password is case-sensitive: Kalmar9
Check-in: Added password protection. The password is case-sensitive: Kalmar9


Question: What are some options for hosting this SPA on a website, where it is publicly accessible but requires a password be used to access it?

Netlify
Vercel
Cloudflare
Github Pages + custom solution
Self-host on your own server