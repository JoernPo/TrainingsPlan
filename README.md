<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-title" content="Powerbuilding">
    <link rel="manifest" href="manifest.webmanifest">
    <link rel="apple-touch-icon" href="icons/apple-touch-icon.png">
    <link rel="icon" href="icons/icon-192.png">
    <meta name="theme-color" content="#e8590c">
    <meta name="apple-mobile-web-app-status-bar-style" content="default">
    <title>Powerbuilding Planner</title>
    <style>
        :root {
            --bg: #f4f5f7;
            --card: #fff;
            --tx: #14171c;
            --mu: #6b7380;
            --ln: #e3e6ea;
            --ac: #e8590c;
            --act: #fff;
            --tint: #fff1e8;
            --cur: #2b8a3e;
            box-sizing: border-box;
            padding-top: env(safe-area-inset-top,0px);
            padding-bottom: env(safe-area-inset-bottom,0px)
        }

        @media (prefers-color-scheme:dark) {
            :root:not([data-theme="light"]) {
                --bg: #101215;
                --card: #1a1d22;
                --tx: #eef0f3;
                --mu: #98a1ae;
                --ln: #2a2f36;
                --ac: #ff8a3d;
                --act: #111;
                --tint: #2b1d12;
                --cur: #51cf66
            }
        }

        :root[data-theme="dark"] {
            --bg: #101215;
            --card: #1a1d22;
            --tx: #eef0f3;
            --mu: #98a1ae;
            --ln: #2a2f36;
            --ac: #ff8a3d;
            --act: #111;
            --tint: #2b1d12;
            --cur: #51cf66
        }

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent
        }

        html {
            scroll-padding-top: env(safe-area-inset-top,0px)
        }

        body {
            margin: 0;
            background: var(--bg);
            color: var(--tx);
            font: 14px/1.35 -apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Helvetica,Arial,sans-serif;
            max-width: 640px;
            margin: 0 auto;
            padding-bottom: 40px
        }

        header {
            position: sticky;
            top: env(safe-area-inset-top,0px);
            z-index: 5;
            background: var(--bg);
            display: flex;
            gap: 6px;
            padding: 8px 10px;
            border-bottom: 1px solid var(--ln)
        }

        button, select {
            font: inherit;
            color: var(--tx);
            background: var(--card);
            border: 1px solid var(--ln);
            border-radius: 10px;
            height: 40px;
            padding: 0 12px
        }

            button:active {
                opacity: .6
            }

        #sel {
            flex: 1;
            font-weight: 700;
            min-width: 0
        }

        .sub {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 8px 12px;
            gap: 8px
        }

            .sub small {
                color: var(--mu)
            }

        #mark {
            height: 34px;
            font-size: 13px
        }

            #mark.on {
                background: var(--cur);
                color: var(--act);
                border-color: var(--cur);
                font-weight: 700
            }

        .day {
            margin: 8px 8px 0;
            background: var(--card);
            border: 1px solid var(--ln);
            border-radius: 12px;
            overflow: hidden
        }

        summary {
            padding: 9px 12px;
            font-weight: 700;
            cursor: pointer;
            list-style: none;
            display: flex;
            justify-content: space-between;
            color: var(--ac)
        }

            summary::-webkit-details-marker {
                display: none
            }

            summary::after {
                content: "▾";
                color: var(--mu)
            }

        details:not([open]) > summary::after {
            content: "▸"
        }

        .ex {
            padding: 7px 12px;
            border-top: 1px solid var(--ln)
        }

        .nm {
            font-weight: 600;
            font-size: 14.5px
        }

        .cells {
            display: flex;
            flex-wrap: wrap;
            gap: 4px 6px;
            margin-top: 4px
        }

        .c {
            min-width: 44px;
            padding: 2px 7px;
            border-radius: 7px;
            background: var(--bg);
            text-align: center
        }

            .c i {
                display: block;
                font-style: normal;
                font-size: 9.5px;
                text-transform: uppercase;
                letter-spacing: .03em;
                color: var(--mu)
            }

            .c b {
                font-weight: 600;
                font-size: 14px
            }

            .c.l {
                background: var(--tint)
            }

                .c.l b {
                    color: var(--ac);
                    font-size: 15px
                }

        .nt {
            margin-top: 4px;
            color: var(--mu);
            font-size: 12.5px
        }

        .rest, .wn {
            margin: 8px 8px 0;
            padding: 8px 12px;
            border-radius: 12px;
            color: var(--mu);
            font-size: 12.5px
        }

        .rest {
            border: 1px dashed var(--ln);
            text-align: center
        }

        .wn {
            background: var(--tint);
            white-space: pre-wrap
        }

            .wn summary {
                padding: 0;
                color: var(--ac)
            }

        #sheet {
            position: fixed;
            inset: 0;
            z-index: 10;
            background: var(--bg);
            overflow: auto;
            padding: env(safe-area-inset-top,0px) 0 env(safe-area-inset-bottom,0px)
        }

            #sheet[hidden] {
                display: none
            }

            #sheet h2 {
                margin: 0;
                font-size: 17px
            }

        .sh {
            position: sticky;
            top: 0;
            background: var(--bg);
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 12px;
            border-bottom: 1px solid var(--ln)
        }

        .sec {
            margin: 12px 10px;
            background: var(--card);
            border: 1px solid var(--ln);
            border-radius: 12px;
            padding: 6px 12px
        }

            .sec h3 {
                margin: 8px 0 2px;
                font-size: 12px;
                text-transform: uppercase;
                color: var(--mu);
                letter-spacing: .05em
            }

        .row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 10px;
            padding: 7px 0;
            border-top: 1px solid var(--ln)
        }

            .row:first-of-type {
                border-top: 0
            }

            .row input[type=text] {
                width: 120px;
                height: 36px;
                border: 1px solid var(--ln);
                border-radius: 8px;
                padding: 0 8px;
                background: var(--bg);
                color: var(--tx);
                font: inherit;
                text-align: right
            }

            .row input[type=checkbox] {
                width: 22px;
                height: 22px;
                accent-color: var(--ac)
            }

            .row small {
                color: var(--mu);
                display: block;
                font-size: 11px
            }

        .seg {
            display: flex
        }

            .seg button {
                border-radius: 0;
                height: 34px
            }

                .seg button:first-child {
                    border-radius: 8px 0 0 8px
                }

                .seg button:last-child {
                    border-radius: 0 8px 8px 0
                }

            .seg .on {
                background: var(--ac);
                color: var(--act);
                border-color: var(--ac)
            }

        #calc[hidden] {
            display: none
        }

        #calc .row input {
            width: 110px
        }

        .res {
            display: flex;
            justify-content: space-between;
            align-items: baseline;
            padding: 7px 0;
            border-top: 1px solid var(--ln)
        }

            .res b {
                font-size: 18px;
                color: var(--ac)
            }

            .res small {
                color: var(--mu)
            }

        #cbtn {
            font-weight: 700;
            font-size: 13px
        }

            #cbtn.on {
                background: var(--ac);
                color: var(--act);
                border-color: var(--ac)
            }

        .fv {
            border: 0;
            background: none;
            height: auto;
            width: auto;
            padding: 0 6px 0 0;
            font-size: 17px;
            line-height: 1;
            color: var(--mu);
            vertical-align: -1px
        }

            .fv.on {
                color: var(--ac)
            }

        .row .fv {
            font-size: 21px;
            padding: 2px 4px
        }

        .row .g {
            flex: 1;
            min-width: 0
        }

        #q {
            width: 100%;
            height: 38px;
            border: 1px solid var(--ln);
            border-radius: 8px;
            padding: 0 10px;
            background: var(--bg);
            color: var(--tx);
            font: inherit;
            margin: 6px 0
        }

        #toast {
            position: fixed;
            left: 50%;
            bottom: calc(20px + env(safe-area-inset-bottom,0px));
            transform: translateX(-50%);
            background: var(--tx);
            color: var(--bg);
            padding: 10px 16px;
            border-radius: 20px;
            z-index: 20;
            font-weight: 600
        }
    </style>
</head>
<body>
    <header><button id="prev" aria-label="Previous week">‹</button><select id="sel" aria-label="Week"></select><button id="next" aria-label="Next week">›</button><button id="cbtn" aria-label="1RM calculator">1RM</button><button id="gear" aria-label="Settings">⚙︎</button></header>
    <section id="calc" class="sec" hidden><h3>1RM calculator</h3><div class="row"><span>Weight</span><input type="text" id="cw" inputmode="decimal" placeholder="e.g. 100"></div><div class="row"><span>Reps</span><input type="text" id="cr" inputmode="numeric" placeholder="e.g. 5"></div><div id="cres"></div></section>
    <div class="sub"><small id="pos"></small><button id="mark"></button></div>
    <main id="main"></main>
    <div id="sheet" hidden></div>
    <script>
        const D = [{ "n": "Week 1", "d": [{ "n": "Full Body 1: Squat, OHP", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "1", "r": "5", "q": "7.5", "t": "3-4 min", "o": "Focus on technique and explosive power!", "c": "75-80%", "b": "Squat", "p": [0.75, 0.8] }, { "n": "Back squat", "w": "0", "s": "2", "r": "8", "t": "3-4 min", "o": "Keep back angle and form consistent across all reps", "c": "70%", "b": "Squat", "p": [0.7] }, { "n": "Overhead press", "w": "2", "s": "3", "r": "8", "t": "2-3 min", "o": "Reset each rep (don't touch-and-press)", "c": "70%", "b": "OHP", "p": [0.7] }, { "n": "Glute Ham Raise", "w": "1", "s": "3", "r": "8-10", "q": "7", "t": "1-2 min", "o": "Keep your hips straight, do nordic ham curls if no GHR machine" }, { "n": "Helms row", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Strict form. Drive elbows out and back at 45 degree angle" }, { "n": "Hammer curl", "w": "0", "s": "3", "r": "20-25", "q": "10", "t": "1-2 min", "o": "Keep elbows locked in place, squeeze the dumbbell handle hard!" }] }, { "n": "Full Body 2: Deadlift, Bench Press", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "3", "r": "4", "t": "3-5 min", "o": "Conventional or sumo: use whatever stance you are stronger with", "c": "80%", "b": "Deadlift", "p": [0.8] }, { "n": "Barbell bench press", "w": "4", "s": "1", "r": "3", "q": "8.5", "t": "4-5 min", "o": "Top set. Leave 1 (maybe 2) reps in the tank. Hard set.", "c": "82.5-87.5%", "b": "Bench", "p": [0.825, 0.875] }, { "n": "Barbell bench press", "w": "0", "s": "2", "r": "10", "t": "2-3 min", "o": "Quick 1 second pause on the chest on each rep", "c": "67.5%", "b": "Bench", "p": [0.675] }, { "n": "Hip abduction", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Machine, band or weighted, 1 second isometric hold at the top of each rep" }, { "n": "Weighted pull-up", "w": "1", "s": "3", "r": "5-8", "q": "8", "t": "3-4 min", "o": "1.5x shoulder width grip, pull your chest to the bar" }, { "n": "Standing calf raise", "w": "1", "s": "3", "r": "8-10", "q": "9", "t": "2-3 min", "o": "1-2 second pause at the bottom of each rep, full ROM" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }, { "n": "Full Body 3: Squat, Dip", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "3", "r": "4", "t": "3-4 min", "o": "Maintain tight pressure in your upper back against the bar", "c": "80%", "b": "Squat", "p": [0.8] }, { "n": "Weighted dip", "w": "2", "s": "3", "r": "8", "q": "8", "t": "2-3 min", "o": "Do dumbbell floor press if no access to dip handles" }, { "n": "Hanging leg raise", "w": "0", "s": "3", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Knees to chest, controlled reps, straighten legs more to increase difficulty" }, { "n": "Lat pull-over", "w": "1", "s": "3", "r": "12-15", "q": "8", "t": "1-2 min", "o": "Can use a cable/rope or band, stretch and squeeze lats!" }, { "n": "Incline dumbbell curl", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Do each arm one at a time rather than alternating, start with your weak arm" }, { "n": "Face pull", "w": "0", "s": "4", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Can use cable/rope or band, retract your shoulder blades as you pull" }] }, { "n": "Full Body 4: Deadlift, Bench Press", "r": false, "e": [{ "n": "Pause deadlift", "w": "4", "s": "4", "r": "2", "t": "3-4 min", "o": "3 second pause right after the plates come off the ground", "c": "75%", "b": "Deadlift", "p": [0.75] }, { "n": "Pause barbell bench press", "w": "3", "s": "3", "r": "5", "t": "2-3 min", "o": "2-3 second pause on the chest", "c": "75%", "b": "Bench", "p": [0.75] }, { "n": "Chest-supported T-Bar row OR Pendlay row", "w": "1", "s": "3", "r": "10", "q": "7", "t": "2-3 min", "o": "Be mindful of lower back fatigue. Stay light, minimize cheating" }, { "n": "Nordic ham curl", "w": "0", "s": "3", "r": "6-8", "q": "8", "t": "1-2 min", "o": "See video demos page in the program pdf, can sub for lying leg curl" }, { "n": "Dumbbell shrug", "w": "0", "s": "3", "r": "20-25", "q": "9", "t": "1-2 min", "o": "Feel a stretch on the traps at the bottom, squeeze hard at the top" }] }, { "n": "Full Body 5: Arm & Pump Day", "r": false, "e": [{ "n": "A1. Barbell or EZ bar curl", "w": "1", "s": "3", "r": "12", "q": "8", "t": "30sec", "o": "Curl the bar out and up in an arc. Minimize momentum." }, { "n": "A2. Floor skull crusher", "w": "1", "s": "3", "r": "12", "q": "8", "t": "30sec", "o": "Arc the bar back behind your head, soft touch on the floor behind you" }, { "n": "B1. Incline dumbbell curl (reverse 21's)", "w": "0", "s": "3", "r": "21", "q": "10", "t": "30sec", "o": "Do both arms at once: 7 reps full ROM, 7 reps top 1/2, 7 reps bottom 1/2" }, { "n": "B2. Triceps pressdown (reverse 21's)", "w": "0", "s": "3", "r": "21", "q": "10", "t": "30sec", "o": "Do both arms at once: 7 reps full ROM, 7 reps bottom 1/2, 7 reps top 1/2" }, { "n": "C1. Dumbbell lateral raise", "w": "0", "s": "3", "r": "20", "q": "9", "t": "30sec", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "C2. Band pull-apart", "w": "0", "s": "3", "r": "20", "q": "9", "t": "30sec", "o": "Mind-muscle connection with rear delts" }, { "n": "C3. Standing calf raise", "w": "0", "s": "3", "r": "12", "q": "9", "t": "30sec", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }, { "n": "C4. Bicycle crunch", "w": "0", "s": "3", "r": "15", "q": "9", "t": "30sec", "o": "Focus on rounding your back as you crunch hard!" }, { "n": "Neck flexion/extension (optional)", "w": "1", "s": "3", "r": "15/15", "q": "8", "t": "1-2 min", "o": "Avoid yanking the plate with your hands" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }], "o": [] }, { "n": "Week 2", "d": [{ "n": "Lower #1", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "3", "r": "3", "t": "3-4 min", "o": "Brace your lats, chest tall, pull the slack out of the bar before lifting", "c": "80%", "b": "Deadlift", "p": [0.8] }, { "n": "Sumo box squat or pause high-bar squat", "w": "2", "s": "2", "r": "8", "q": "7", "t": "2-3 min", "o": "If you squat high-bar, do sumo box squat. If you squat low-bar, do pause high-bar (2 sec pause)" }, { "n": "Pull-through", "w": "0", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Can use cable/rope or band, use your glutes to move the weight" }, { "n": "Leg curl", "w": "1", "s": "3", "r": "6-8", "q": "8", "t": "1-2 min", "o": "Do lying leg curl machine or nordic ham curl if no machine access" }, { "n": "Standing calf raise", "w": "1", "s": "3", "r": "8-10", "q": "9", "t": "1-2 min", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }] }, { "n": "Upper #1", "r": false, "e": [{ "n": "Barbell bench press", "w": "4", "s": "1", "r": "2", "q": "8", "t": "4-5 min", "o": "Top set. Leave ~2 reps in the tank. Hard set.", "c": "85-90%", "b": "Bench", "p": [0.85, 0.9] }, { "n": "Barbell bench press", "w": "0", "s": "3", "r": "6", "t": "2-3 min", "o": "Set up a comfortable arch, slight pause on the chest, explode up", "c": "77.5%", "b": "Bench", "p": [0.775] }, { "n": "Chin-up", "w": "1", "s": "3", "r": "8-10", "q": "8", "t": "2-3 min", "o": "Underhand grip, pull your chest to the bar, add weight if needed to hit RPE" }, { "n": "Standing arnold dumbbell press", "w": "1", "s": "2", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Rotate the DBs in at the bottom and out at the top" }, { "n": "Chest-supported dumbbell row", "w": "1", "s": "2", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Lie on an incline bench and do rows - pull with lats!" }, { "n": "Face pull", "w": "0", "s": "2", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Can use cable/rope or band, retract your shoulder blades as you pull" }, { "n": "Dumbbell lateral raise", "w": "0", "s": "2", "r": "15-20", "q": "10", "t": "1-2 min", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "Concentration bicep curl", "w": "0", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Pin your elbow against your upper leg or the back of a bench" }] }, { "n": "Lower #2", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "3", "r": "6", "t": "3-4 min", "o": "Sit back and down, keep your upper back tight to the bar", "c": "75%", "b": "Squat", "p": [0.75] }, { "n": "Good morning", "w": "2", "s": "2", "r": "10-12", "q": "7", "t": "2-3 min", "o": "Same as squat stance, keep shins straight, go lighter and \"feel\" hamstrings" }, { "n": "Leg extension", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Use bands if no machine access, mind-muscle connection with quads" }, { "n": "Standing calf raise", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Emphasize the mind-muscle connection" }, { "n": "Banded lateral walk or hip abduction", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Point toes slightly outward, mind-muscle connection with glutes" }, { "n": "V sit-up", "w": "0", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Think about squeezing your upper and lower abs together" }] }, { "n": "Upper #2", "r": false, "e": [{ "n": "Overhead press", "w": "3", "s": "3", "r": "4", "t": "3-4 min", "o": "Squeeze your glutes to keep your torso upright, press up and slightly back", "c": "80%", "b": "OHP", "p": [0.8] }, { "n": "Single-arm lat pulldown", "w": "1", "s": "2", "r": "10-12", "q": "9", "t": "2-3 min", "o": "Perform with bands if no lat pulldown, drive elbows down and in" }, { "n": "Close-grip bench press", "w": "2", "s": "2", "r": "12", "q": "7", "t": "2-3 min", "o": "Shoulder width grip, tuck your elbows in closer to your torso" }, { "n": "Pendlay row", "w": "1", "s": "2", "r": "10", "q": "7", "t": "1-2 min", "o": "Be mindful of lower back fatigue. Stay light, minimize cheating" }, { "n": "Pec flye", "w": "0", "s": "2", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Perform with cable, bands or dumbbells, use a full ROM" }, { "n": "A1. Incline shrug", "w": "1", "s": "2", "r": "15-20", "q": "9", "t": "30sec", "o": "Lie face down against an incline bench and do shrugs - full ROM and squeeze!" }, { "n": "A2. Upright row", "w": "1", "s": "2", "r": "15-20", "q": "9", "t": "30sec", "o": "Can use cables/rope, bands or dumbbells. Stop ROM once elbows reach shoulder height." }, { "n": "Barbell skull crusher", "w": "1", "s": "2", "r": "8-10", "q": "8", "t": "1-2 min", "o": "Do these on a bench, constant tension on triceps" }] }, { "n": "Lower #3", "r": false, "e": [{ "n": "5\" block pull", "w": "4", "s": "2", "r": "4", "q": "8", "t": "4-5 min", "o": "Do block pulls from a 5\" block (can stack 45lb + 10lb bumper plates as blocks)" }, { "n": "Bulgarian split squat", "w": "1", "s": "2", "r": "12", "q": "7", "t": "2-3 min", "o": "12 reps each leg, keep your torso upright, constant-tension on quads" }, { "n": "Barbell 45° hyperextension or hip thrust", "w": "1", "s": "2", "r": "8-10", "q": "7", "t": "1-2 min", "o": "Do barbell hip thrusts if no machine, use glutes to move the weight" }, { "n": "Seated calf raise", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Do standing if no machine, emphasize the mind-muscle connection" }, { "n": "Hanging leg raise", "w": "0", "s": "3", "r": "10-12", "q": "8", "t": "1-2 min", "o": "Knees to chest, controlled reps, straighten legs more to increase difficulty" }, { "n": "Neck flexion/extension (optional)", "w": "1", "s": "3", "r": "12/12", "q": "8", "t": "1-2 min", "o": "12 reps flexion (front of neck), 12 reps extension (back of neck)" }] }, { "n": "Upper #3", "r": false, "e": [{ "n": "\"Flat-back\" barbell bench press", "w": "3", "s": "3", "r": "10", "q": "7", "t": "2-3 min", "o": "Shoulder blades still retracted and depressed. Slight arch in upper back. Minimize leg drive." }, { "n": "Eccentric-accentuated pull-up", "w": "1", "s": "2", "r": "AMRAP", "q": "10", "t": "2-3 min", "o": "3 second negative on every rep, maintain controlled form for all reps" }, { "n": "Weighted dip", "w": "2", "s": "2", "r": "10", "q": "8", "t": "2-3 min", "o": "Do dumbbell floor press if no access to dip handles" }, { "n": "Single-arm row", "w": "1", "s": "2", "r": "10-12", "q": "9", "t": "2-3 min", "o": "Can use cables, bands or dumbbells - feel your lats working!" }, { "n": "Barbell or EZ bar curl", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Focus on the mind-muscle connection" }, { "n": "Lean-away lateral raise", "w": "0", "s": "2", "r": "30", "q": "10", "t": "1-2 min", "o": "Use a light dumbbell, constant-tension, no pause at the bottom" }, { "n": "Bicycle crunch", "w": "0", "s": "2", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Keep your hands behind your ears" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }], "o": [] }, { "n": "Week 3", "d": [{ "n": "Full Body 1: Squat, OHP", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "1", "r": "8", "q": "8.5", "t": "4-5 min", "o": "Top set. Leave 1 (maybe 2) reps in the tank. Push it!", "c": "72.5-77.5%", "b": "Squat", "p": [0.725, 0.775] }, { "n": "Back squat", "w": "0", "s": "2", "r": "6", "t": "3-4 min", "o": "Keep back angle and form consistent across all reps", "c": "75%", "b": "Squat", "p": [0.75] }, { "n": "Overhead press", "w": "2", "s": "3", "r": "8", "t": "2-3 min", "o": "Reset each rep (don't touch-and-press)", "c": "72.5%", "b": "OHP", "p": [0.725] }, { "n": "Glute Ham Raise", "w": "1", "s": "2", "r": "8-10", "q": "7", "t": "1-2 min", "o": "Keep your hips straight, do nordic ham curls if no GHR machine" }, { "n": "Helms row", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Strict form. Drive elbows out and back at 45 degree angle" }, { "n": "Hammer curl", "w": "0", "s": "3", "r": "20-25", "q": "10", "t": "1-2 min", "o": "Keep elbows locked in place, squeeze the dumbbell handle hard!" }] }, { "n": "Full Body 2: Deadlift, Bench Press", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "4", "r": "2", "t": "3-5 min", "o": "Conventional or sumo: use whatever stance you are stronger with", "c": "85%", "b": "Deadlift", "p": [0.85] }, { "n": "Barbell bench press", "w": "3", "s": "1", "r": "6", "q": "8.5", "t": "4-5 min", "o": "Top set. Leave 1 (maybe 2) reps in the tank. Push it!", "c": "75-80%", "b": "Bench", "p": [0.75, 0.8] }, { "n": "Barbell bench press", "w": "0", "s": "2", "r": "8", "t": "2-3 min", "o": "Quick 1 second pause on the chest on each rep", "c": "72.5%", "b": "Bench", "p": [0.725] }, { "n": "Hip abduction", "w": "0", "s": "2", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Machine, band or weighted, 1 second isometric hold at the top of each rep" }, { "n": "Weighted pull-up", "w": "1", "s": "3", "r": "5-8", "q": "8", "t": "3-4 min", "o": "1.5x shoulder width grip, pull your chest to the bar" }, { "n": "Standing calf raise", "w": "1", "s": "3", "r": "8", "q": "9", "t": "2-3 min", "o": "1-2 second pause at the bottom of each rep, full ROM" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }, { "n": "Full Body 3: Squat, Dip", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "4", "r": "4", "t": "3-4 min", "o": "Maintain tight pressure in your upper back against the bar", "c": "80%", "b": "Squat", "p": [0.8] }, { "n": "Weighted dip", "w": "2", "s": "3", "r": "8", "q": "8", "t": "2-3 min", "o": "Do dumbbell floor press if no access to dip handles" }, { "n": "Hanging leg raise", "w": "0", "s": "3", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Knees to chest, controlled reps, straighten legs more to increase difficulty" }, { "n": "Lat pull-over", "w": "1", "s": "3", "r": "12-15", "q": "8", "t": "1-2 min", "o": "Can use a DB, cable/rope or band, stretch and squeeze lats!" }, { "n": "Incline dumbbell curl", "w": "1", "s": "2", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Do each arm one at a time rather than alternating, start with your weak arm" }, { "n": "Face pull", "w": "0", "s": "4", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Can use cable/rope or band, retract your shoulder blades as you pull" }] }, { "n": "Full Body 4: Deadlift, Bench Press", "r": false, "e": [{ "n": "Pause deadlift", "w": "4", "s": "4", "r": "2", "t": "3-4 min", "o": "3 second pause right after the plates come off the ground", "c": "77.5%", "b": "Deadlift", "p": [0.775] }, { "n": "Pause barbell bench press", "w": "3", "s": "4", "r": "5", "t": "2-3 min", "o": "2-3 second pause on the chest", "c": "75%", "b": "Bench", "p": [0.75] }, { "n": "Chest-supported T-Bar row OR Pendlay row", "w": "1", "s": "3", "r": "10", "q": "7", "t": "2-3 min", "o": "Be mindful of lower back fatigue. Stay light, minimize cheating" }, { "n": "Nordic ham curl", "w": "0", "s": "2", "r": "6-8", "q": "8", "t": "1-2 min", "o": "See video demos page in the program pdf, can sub for lying leg curl" }, { "n": "Dumbbell shrug", "w": "0", "s": "3", "r": "20-25", "q": "9", "t": "1-2 min", "o": "Feel a stretch on the traps at the bottom, squeeze hard at the top" }] }, { "n": "Full Body 5: Arm & Pump Day", "r": false, "e": [{ "n": "A1. Barbell or EZ bar curl", "w": "1", "s": "3", "r": "12", "q": "8", "t": "30sec", "o": "Curl the bar out and up in an arc. Minimize momentum." }, { "n": "A2. Floor skull crusher", "w": "1", "s": "3", "r": "12", "q": "8", "t": "30sec", "o": "Arc the bar back behind your head, soft touch on the floor behind you" }, { "n": "B1. Incline dumbbell curl (reverse 21's)", "w": "0", "s": "3", "r": "21", "q": "10", "t": "30sec", "o": "Do both arms at once: 7 reps full ROM, 7 reps top 1/2, 7 reps bottom 1/2" }, { "n": "B2. Triceps pressdown (reverse 21's)", "w": "0", "s": "3", "r": "21", "q": "10", "t": "30sec", "o": "Do both arms at once: 7 reps full ROM, 7 reps bottom 1/2, 7 reps top 1/2" }, { "n": "C1. Dumbbell lateral raise", "w": "0", "s": "3", "r": "20", "q": "9", "t": "30sec", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "C2. Band pull-apart", "w": "0", "s": "3", "r": "20", "q": "9", "t": "30sec", "o": "Mind-muscle connection with rear delts" }, { "n": "C3. Standing calf raise", "w": "0", "s": "3", "r": "12", "q": "9", "t": "30sec", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }, { "n": "C4. Bicycle crunch", "w": "0", "s": "3", "r": "15", "q": "9", "t": "30sec", "o": "Focus on rounding your back as you crunch hard!" }, { "n": "Neck flexion/extension (optional)", "w": "1", "s": "3", "r": "15/15", "q": "8", "t": "1-2 min", "o": "Avoid yanking the plate with your hands" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }], "o": [] }, { "n": "Week 4", "d": [{ "n": "Lower #1", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "1", "r": "2", "q": "9", "t": "4-5 min", "o": "Top set! Aim for near PR. Keep form tight.", "c": "87.5-92.5%", "b": "Deadlift", "p": [0.875, 0.925] }, { "n": "Deadlift", "w": "0", "s": "3", "r": "3", "t": "3-4 min", "o": "Brace your lats, chest tall, pull the slack out of the bar before lifting", "c": "80%", "b": "Deadlift", "p": [0.8] }, { "n": "Sumo box squat or pause high-bar squat", "w": "2", "s": "2", "r": "8", "q": "7", "t": "2-3 min", "o": "If you squat high-bar, do sumo box squat. If you squat low-bar, do pause high-bar (2 sec pause)" }, { "n": "Pull-through", "w": "0", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Can use cable/rope or band, use your glutes to move the weight" }, { "n": "Leg curl", "w": "1", "s": "3", "r": "6-8", "q": "8", "t": "1-2 min", "o": "Do lying leg curl machine or nordic ham curl if no machine access" }, { "n": "Standing calf raise", "w": "1", "s": "3", "r": "8-10", "q": "9", "t": "1-2 min", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }] }, { "n": "Upper #1", "r": false, "e": [{ "n": "Barbell bench press", "w": "3", "s": "3", "r": "6", "t": "3-4 min", "o": "Set up a comfortable arch, slight pause on the chest, explode up", "c": "77.5%", "b": "Bench", "p": [0.775] }, { "n": "Chin-up", "w": "1", "s": "3", "r": "8-10", "q": "8", "t": "2-3 min", "o": "Underhand grip, pull your chest to the bar, add weight if needed to hit RPE" }, { "n": "Standing arnold dumbbell press", "w": "1", "s": "2", "r": "10-12", "q": "9", "t": "2-3 min", "o": "Rotate the DBs in at the bottom and out at the top" }, { "n": "Chest-supported dumbbell row", "w": "1", "s": "2", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Lie on an incline bench and do rows - pull with lats!" }, { "n": "Face pull", "w": "0", "s": "2", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Can use cable/rope or band, retract your shoulder blades as you pull" }, { "n": "Dumbbell lateral raise", "w": "0", "s": "2", "r": "15-20", "q": "10", "t": "1-2 min", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "Concentration bicep curl", "w": "0", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Pin your elbow against your upper leg or the back of a bench" }] }, { "n": "Lower #2", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "3", "r": "6", "t": "3-4 min", "o": "Sit back and down, keep your upper back tight to the bar", "c": "75%", "b": "Squat", "p": [0.75] }, { "n": "Good morning", "w": "2", "s": "2", "r": "10-12", "q": "7", "t": "2-3 min", "o": "50kg x80" }, { "n": "Leg extension", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Use bands if no machine access, mind-muscle connection with quads" }, { "n": "Standing calf raise", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Emphasize the mind-muscle connection" }, { "n": "Banded lateral walk or hip abduction", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Point toes slightly outward, mind-muscle connection with glutes" }, { "n": "V sit-up", "w": "0", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Think about squeezing your upper and lower abs together" }] }, { "n": "Upper #2", "r": false, "e": [{ "n": "Overhead press / push press", "w": "3", "s": "3", "r": "3/3", "t": "3-4 min", "o": "First 3 reps strict military press (no leg drive), last 3 reps push press (use leg drive)", "c": "80%", "b": "OHP", "p": [0.8] }, { "n": "Single-arm lat pulldown", "w": "1", "s": "2", "r": "10-12", "q": "9", "t": "2-3 min", "o": "Perform with bands if no lat pulldown, drive elbows down and in" }, { "n": "Barbell floor press", "w": "2", "s": "2", "r": "12", "q": "7", "t": "1-2 min", "o": "Control the eccentric (don't let your elbows slam into the ground), be explosive on the way up" }, { "n": "Pendlay row", "w": "1", "s": "2", "r": "10", "q": "7", "t": "1-2 min", "o": "Be mindful of lower back fatigue. Stay light, minimize cheating" }, { "n": "Pec flye", "w": "0", "s": "2", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Perform with cable, bands or dumbbells, use a full ROM" }, { "n": "A1. Incline shrug", "w": "1", "s": "2", "r": "15-20", "q": "9", "t": "30sec", "o": "Lie face down against an incline bench and do shrugs - full ROM and squeeze!" }, { "n": "A2. Bent over reverse dumbbell flye", "w": "1", "s": "2", "r": "15-20", "q": "9", "t": "30sec", "o": "Mind-muscle connection with rear delts, sweep the weight out" }, { "n": "Barbell skull crusher", "w": "1", "s": "2", "r": "8-10", "q": "8", "t": "1-2 min", "o": "Do these on a bench, constant tension on triceps" }] }, { "n": "Lower #3", "r": false, "e": [{ "n": "4\" block pull", "w": "4", "s": "2", "r": "4", "q": "8", "t": "4-5 min", "o": "Do block pulls from a 4\" block (can use 45lb bumper plate as a block)" }, { "n": "Bulgarian split squat", "w": "1", "s": "2", "r": "12", "q": "7", "t": "2-3 min", "o": "12 reps each leg, keep your torso upright, constant-tension on quads" }, { "n": "Barbell 45° hyperextension or hip thrust", "w": "1", "s": "2", "r": "8-10", "q": "7", "t": "1-2 min", "o": "Do barbell hip thrusts if no machine, use glutes to move the weight" }, { "n": "Seated calf raise", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Do standing if no machine, emphasize the mind-muscle connection" }, { "n": "Hanging leg raise", "w": "0", "s": "3", "r": "10-12", "q": "8", "t": "1-2 min", "o": "Knees to chest, controlled reps, straighten legs more to increase difficulty" }, { "n": "Neck flexion/extension (optional)", "w": "1", "s": "3", "r": "12/12", "q": "8", "t": "1-2 min", "o": "12 reps flexion (front of neck), 12 reps extension (back of neck)" }] }, { "n": "Upper #3", "r": false, "e": [{ "n": "\"Flat-back\" barbell bench press", "w": "3", "s": "3", "r": "10", "q": "7", "t": "2-3 min", "o": "Shoulder blades still retracted and depressed. Slight arch in upper back. Minimize leg drive." }, { "n": "Eccentric-accentuated pull-up", "w": "1", "s": "2", "r": "AMRAP", "q": "10", "t": "2-3 min", "o": "3 second negative on every rep, maintain controlled form for all reps" }, { "n": "Weighted dip", "w": "2", "s": "2", "r": "10", "q": "8", "t": "2-3 min", "o": "Do dumbbell floor press if no access to dip handles" }, { "n": "Single-arm row", "w": "1", "s": "2", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Can use cables, bands or dumbbells - feel your lats working!" }, { "n": "Barbell or EZ bar curl", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Focus on the mind-muscle connection" }, { "n": "Lean-away lateral raise", "w": "0", "s": "2", "r": "30", "q": "10", "t": "1-2 min", "o": "Use a light dumbbell, constant-tension, no pause at the bottom" }, { "n": "Bicycle crunch", "w": "0", "s": "2", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Keep your hands behind your ears" }, { "n": "pendlay row \"22,5\"kg Hanteln 10 mal" }] }], "o": ["SZ40er curls, seated dann 15er hanteln"] }, { "n": "Week 5", "d": [{ "n": "Full Body 1: Squat, OHP", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "1", "r": "3", "q": "8.5", "t": "4-5 min", "o": "Top set. Leave 1 (maybe 2) reps in the tank. Aim for near 3 rep PR.", "c": "82.5-87.5%", "b": "Squat", "p": [0.825, 0.875] }, { "n": "Back squat", "w": "0", "s": "2", "r": "4", "t": "3-4 min", "o": "Keep back angle and form consistent across all reps", "c": "80%", "b": "Squat", "p": [0.8] }, { "n": "Overhead press", "w": "2", "s": "3", "r": "8", "t": "2-3 min", "o": "Reset each rep (don't touch-and-press)", "c": "75%", "b": "OHP", "p": [0.75] }, { "n": "Glute Ham Raise", "w": "1", "s": "2", "r": "8-10", "q": "8", "t": "1-2 min", "o": "Keep your hips straight, do nordic ham curls if no GHR machine" }, { "n": "Helms row", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Strict form. Drive elbows out and back at 45 degree angle" }, { "n": "Hammer curl", "w": "0", "s": "3", "r": "20-25", "q": "10", "t": "1-2 min", "o": "Keep elbows locked in place, squeeze the dumbbell handle hard!" }] }, { "n": "Full Body 2: Deadlift, Bench Press", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "3", "r": "3", "t": "3-5 min", "o": "Brace your lats, chest tall, pull the slack out of the bar before lifting", "c": "85%", "b": "Deadlift", "p": [0.85] }, { "n": "Barbell bench press", "w": "4", "s": "1", "r": "4", "q": "9", "t": "4-5 min", "o": "Top set. Leave 1 rep in the tank. Aim for near 4 rep PR.", "c": "82.5-87.5%", "b": "Bench", "p": [0.825, 0.875] }, { "n": "Barbell bench press", "w": "0", "s": "2", "r": "6", "t": "2-3 min", "o": "Quick 1 second pause on the chest on each rep", "c": "80%", "b": "Bench", "p": [0.8] }, { "n": "Hip abduction", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Machine, band or weighted, 1 second isometric hold at the top of each rep" }, { "n": "Weighted pull-up", "w": "1", "s": "3", "r": "5-8", "q": "8", "t": "3-4 min", "o": "1.5x shoulder width grip, pull your chest to the bar" }, { "n": "Standing calf raise", "w": "1", "s": "3", "r": "8", "q": "9", "t": "2-3 min", "o": "1-2 second pause at the bottom of each rep, full ROM" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }, { "n": "Full Body 3: Squat, Dip", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "3", "r": "6", "t": "3-4 min", "o": "Maintain tight pressure in your upper back against the bar", "c": "77.5%", "b": "Squat", "p": [0.775] }, { "n": "Weighted dip", "w": "2", "s": "3", "r": "8", "q": "8", "t": "2-3 min", "o": "Do dumbbell floor press if no access to dip handles" }, { "n": "Hanging leg raise", "w": "0", "s": "3", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Knees to chest, controlled reps, straighten legs more to increase difficulty" }, { "n": "Lat pull-over", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Can use a DB, cable/rope or band, stretch and squeeze lats!" }, { "n": "Incline dumbbell curl", "w": "1", "s": "2", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Do each arm one at a time rather than alternating, start with your weak arm" }, { "n": "Face pull", "w": "0", "s": "4", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Can use cable/rope or band, retract your shoulder blades as you pull" }] }, { "n": "Full Body 4: Deadlift, Bench Press", "r": false, "e": [{ "n": "Pause deadlift", "w": "4", "s": "4", "r": "2", "t": "3-4 min", "o": "3 second pause right after the plates come off the ground", "c": "82.5%", "b": "Deadlift", "p": [0.825] }, { "n": "Pause barbell bench press", "w": "3", "s": "3", "r": "6", "t": "2-3 min", "o": "2-3 second pause on the chest", "c": "75%", "b": "Bench", "p": [0.75] }, { "n": "Chest-supported T-Bar row OR Pendlay row", "w": "1", "s": "3", "r": "10", "q": "7", "t": "2-3 min", "o": "Be mindful of lower back fatigue. Stay light, minimize cheating" }, { "n": "Nordic ham curl", "w": "0", "s": "3", "r": "6-8", "q": "8", "t": "1-2 min", "o": "See video demos page in the program pdf, can sub for lying leg curl" }, { "n": "Dumbbell shrug", "w": "0", "s": "3", "r": "20-25", "q": "10", "t": "1-2 min", "o": "Feel a stretch on the traps at the bottom, squeeze hard at the top" }] }, { "n": "Full Body 5: Arm & Pump Day", "r": false, "e": [{ "n": "A1. Barbell or EZ bar curl", "w": "1", "s": "3", "r": "12", "q": "8", "t": "30sec", "o": "Curl the bar out and up in an arc. Minimize momentum." }, { "n": "A2. Floor skull crusher", "w": "1", "s": "3", "r": "12", "q": "8", "t": "30sec", "o": "Arc the bar back behind your head, soft touch on the floor behind you" }, { "n": "B1. Incline dumbbell curl (reverse 21s)", "w": "0", "s": "3", "r": "21", "q": "10", "t": "30sec", "o": "Do both arms at once: 7 reps full ROM, 7 reps top 1/2, 7 reps bottom 1/2" }, { "n": "B2. Triceps pressdown (reverse 21s)", "w": "0", "s": "3", "r": "21", "q": "10", "t": "30sec", "o": "Do both arms at once: 7 reps full ROM, 7 reps bottom 1/2, 7 reps top 1/2" }, { "n": "C1. Dumbbell lateral raise", "w": "0", "s": "3", "r": "20", "q": "9", "t": "30sec", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "C2. Band pull-apart", "w": "0", "s": "3", "r": "20", "q": "9", "t": "30sec", "o": "Mind-muscle connection with rear delts" }, { "n": "C3. Standing calf raise", "w": "0", "s": "3", "r": "12", "q": "9", "t": "30sec", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }, { "n": "C4. Bicycle crunch", "w": "0", "s": "3", "r": "15", "q": "9", "t": "30sec", "o": "Focus on rounding your back as you crunch hard!" }, { "n": "Neck flexion/extension (optional)", "w": "1", "s": "3", "r": "15/15", "q": "8", "t": "1-2 min", "o": "Avoid yanking the plate with your hands" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }], "o": [] }, { "n": "Week 6", "d": [{ "n": "Lower #1", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "3", "r": "4", "t": "3-4 min", "o": "Brace your lats, chest tall, pull the slack out of the bar before lifting", "c": "80%", "b": "Deadlift", "p": [0.8] }, { "n": "Sumo box squat or pause high-bar squat", "w": "2", "s": "2", "r": "8", "q": "5", "t": "2-3 min", "o": "If you squat high-bar, do sumo box squat. If you squat low-bar, do pause high-bar (2 sec pause)" }, { "n": "Pull-through", "w": "0", "s": "2", "r": "12-15", "q": "7", "t": "1-2 min", "o": "Can use cable/rope or band, use your glutes to move the weight" }, { "n": "Leg curl", "w": "1", "s": "3", "r": "6-8", "q": "7", "t": "1-2 min", "o": "Do lying leg curl machine or nordic ham curl if no machine access" }, { "n": "Standing calf raise", "w": "1", "s": "2", "r": "8-10", "q": "8", "t": "1-2 min", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }] }, { "n": "Upper #1", "r": false, "e": [{ "n": "Barbell bench press", "w": "3", "s": "2", "r": "7", "t": "3-4 min", "o": "Set up a comfortable arch, slight pause on the chest, explode up", "c": "77.5%", "b": "Bench", "p": [0.775] }, { "n": "Chin-up", "w": "1", "s": "2", "r": "8-10", "q": "6", "t": "2-3 min", "o": "Underhand grip, pull your chest to the bar, add weight if needed to hit RPE" }, { "n": "Standing arnold dumbbell press", "w": "1", "s": "2", "r": "10-12", "q": "6", "t": "2-3 min", "o": "Rotate the DBs in at the bottom and out at the top" }, { "n": "Chest-supported dumbbell row", "w": "1", "s": "2", "r": "12-15", "q": "6", "t": "1-2 min", "o": "Lie on an incline bench and do rows - pull with lats!" }, { "n": "Face pull", "w": "0", "s": "2", "r": "15-20", "q": "8", "t": "1-2 min", "o": "Can use cable/rope or band, retract your shoulder blades as you pull" }, { "n": "Dumbbell lateral raise", "w": "0", "s": "2", "r": "15-20", "q": "8", "t": "1-2 min", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "Concentration bicep curl", "w": "0", "s": "3", "r": "12-15", "q": "8", "t": "1-2 min", "o": "Pin your elbow against your upper leg or the back of a bench" }] }, { "n": "Lower #2", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "1", "r": "1", "q": "9", "t": "4-5 min", "o": "Only tough set this week! Perfect technique!", "c": "90-95%", "b": "Squat", "p": [0.9, 0.95] }, { "n": "Low-bar back squat", "w": "0", "s": "2", "r": "7", "t": "3-4 min", "o": "Sit back and down, keep your upper back tight to the bar", "c": "75%", "b": "Squat", "p": [0.75] }, { "n": "Leg extension", "w": "1", "s": "3", "r": "12-15", "q": "8", "t": "1-2 min", "o": "Use bands if no machine access, mind-muscle connection with quads" }, { "n": "Standing calf raise", "w": "0", "s": "3", "r": "15-20", "q": "8", "t": "1-2 min", "o": "Emphasize the mind-muscle connection" }, { "n": "Banded lateral walk or hip abduction", "w": "0", "s": "3", "r": "15-20", "q": "8", "t": "1-2 min", "o": "Point toes slightly outward, mind-muscle connection with glutes" }, { "n": "V sit-up", "w": "0", "s": "3", "r": "12-15", "q": "8", "t": "1-2 min", "o": "Think about squeezing your upper and lower abs together" }] }, { "n": "Upper #2", "r": false, "e": [{ "n": "Overhead press", "w": "3", "s": "3", "r": "4", "t": "3-4 min", "o": "Squeeze your glutes to keep your torso upright, press up and slightly back", "c": "82.5%", "b": "OHP", "p": [0.825] }, { "n": "Single-arm lat pulldown", "w": "1", "s": "2", "r": "10-12", "q": "7", "t": "2-3 min", "o": "Perform with bands if no lat pulldown, drive elbows down and in" }, { "n": "Deficit push-up", "w": "2", "s": "2", "r": "AMRAP", "q": "10", "t": "2-3 min", "o": "As many reps as possible. Use Perfect Push-up handles or dumbbells to create a deficit" }, { "n": "Pendlay row", "w": "1", "s": "2", "r": "10", "q": "7", "t": "1-2 min", "o": "Be mindful of lower back fatigue. Stay light, minimize cheating" }, { "n": "Pec flye", "w": "0", "s": "2", "r": "15-20", "q": "7", "t": "1-2 min", "o": "Perform with cable, bands or dumbbells, use a full ROM" }, { "n": "A1. Incline shrug", "w": "1", "s": "2", "r": "15-20", "q": "7", "t": "30sec", "o": "Lie face down against an incline bench and do shrugs - full ROM and squeeze!" }, { "n": "A2. Upright row", "w": "1", "s": "2", "r": "15-20", "q": "7", "t": "30sec", "o": "Can use cables/rope, bands or dumbbells. Stop ROM once elbows reach shoulder height." }, { "n": "Barbell skull crusher", "w": "1", "s": "2", "r": "8-10", "q": "7", "t": "1-2 min", "o": "Do these on a bench, constant tension on triceps" }] }, { "n": "Lower #3", "r": false, "e": [{ "n": "3\" block pull", "w": "4", "s": "2", "r": "4", "q": "6", "t": "4-5 min", "o": "Do block pulls from a 3\" block (can use 25lb + 10lb bumper plates as blocks)" }, { "n": "Bulgarian split squat", "w": "1", "s": "2", "r": "12", "q": "6", "t": "2-3 min", "o": "12 reps each leg, keep your torso upright, constant-tension on quads" }, { "n": "Barbell 45° hyperextension or hip thrust", "w": "1", "s": "2", "r": "8-10", "q": "6", "t": "1-2 min", "o": "Do barbell hip thrusts if no machine, use glutes to move the weight" }, { "n": "Seated calf raise", "w": "0", "s": "3", "r": "15-20", "q": "8", "t": "1-2 min", "o": "Do standing if no machine, emphasize the mind-muscle connection" }, { "n": "Hanging leg raise", "w": "0", "s": "3", "r": "10-12", "q": "8", "t": "1-2 min", "o": "Knees to chest, controlled reps, straighten legs more to increase difficulty" }, { "n": "Neck flexion/extension (optional)", "w": "1", "s": "3", "r": "12/12", "q": "8", "t": "1-2 min", "o": "12 reps flexion (front of neck), 12 reps extension (back of neck)" }] }, { "n": "Upper #3", "r": false, "e": [{ "n": "\"Flat-back\" barbell bench press", "w": "3", "s": "3", "r": "10", "q": "7", "t": "2-3 min", "o": "Shoulder blades still retracted and depressed. Slight arch in upper back. Minimize leg drive." }, { "n": "Neutral grip pull-up", "w": "1", "s": "2", "r": "10", "q": "7", "t": "2-3 min", "o": "Avoid failure, focus on good technique and maintaining consistent tempo" }, { "n": "Weighted dip", "w": "2", "s": "2", "r": "10", "q": "7", "t": "2-3 min", "o": "Do dumbbell floor press if no access to dip handles" }, { "n": "Single-arm row", "w": "1", "s": "2", "r": "10-12", "q": "7", "t": "1-2 min", "o": "Can use cables, bands or dumbbells - feel your lats working!" }, { "n": "Barbell or EZ bar curl", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Focus on the mind-muscle connection" }, { "n": "Lean-away lateral raise", "w": "0", "s": "2", "r": "30", "q": "9", "t": "1-2 min", "o": "Use a light dumbbell, constant-tension, no pause at the bottom" }, { "n": "Bicycle crunch", "w": "0", "s": "2", "r": "10-12", "q": "8", "t": "1-2 min", "o": "Keep your hands behind your ears" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }], "o": ["Semi-deload Week: Avoid failure and train lighter this week to promote recovery and to prepare for the next 4 weeks!"] }, { "n": "Week 7", "d": [{ "n": "Full Body 1: Squat, OHP", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "1", "r": "3", "q": "8.5", "t": "4-5 min", "o": "Try to add some weight from Week 5 or improve bar speed at same weight", "c": "85-90%", "b": "Squat", "p": [0.85, 0.9] }, { "n": "Back squat", "w": "0", "s": "2", "r": "2", "t": "3-4 min", "o": "Be mindful of technique. Focus on driving your back into the bar.", "c": "85%", "b": "Squat", "p": [0.85] }, { "n": "Overhead press", "w": "2", "s": "4", "r": "8", "q": "7", "t": "2-3 min", "o": "Reset each rep (don't touch-and-press)", "c": "70%", "b": "OHP", "p": [0.7] }, { "n": "Glute Ham Raise", "w": "1", "s": "2", "r": "8-10", "q": "8", "t": "1-2 min", "o": "Keep your hips straight, do nordic ham curls if no GHR machine" }, { "n": "Helms row", "w": "1", "s": "2", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Strict form. Drive elbows out and back at 45 degree angle" }, { "n": "Hammer curl", "w": "0", "s": "3", "r": "20-25", "q": "10", "t": "1-2 min", "o": "Keep elbows locked in place, squeeze the dumbbell handle hard!" }] }, { "n": "Full Body 2: Deadlift, Bench Press", "r": false, "e": [{ "n": "Pause deadlift", "w": "4", "s": "4", "r": "2", "t": "3-5 min", "o": "3 second pause right after the plates come off the ground", "c": "75%", "b": "Deadlift", "p": [0.75] }, { "n": "Barbell bench press", "w": "4", "s": "1", "r": "3", "q": "9", "t": "4-5 min", "o": "Top set. Leave 1 rep in the tank. Aim for near 3 rep PR.", "c": "85-90%", "b": "Bench", "p": [0.85, 0.9] }, { "n": "Barbell bench press", "w": "0", "s": "2", "r": "4", "t": "2-3 min", "o": "Focus on technique. Press the bar back and up with explosive force", "c": "80%", "b": "Bench", "p": [0.8] }, { "n": "Hip abduction", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Machine, band or weighted, 1 second isometric hold at the top of each rep" }, { "n": "Weighted pull-up", "w": "1", "s": "3", "r": "3-5", "q": "7", "t": "3-4 min", "o": "1.5x shoulder width grip, pull your chest to the bar" }, { "n": "Standing calf raise", "w": "1", "s": "3", "r": "8", "q": "9", "t": "2-3 min", "o": "1-2 second pause at the bottom of each rep, full ROM" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }, { "n": "Full Body 3: Squat, Dip", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "4", "r": "6", "t": "3-4 min", "o": "Maintain tight pressure in your upper back against the bar", "c": "77.5%", "b": "Squat", "p": [0.775] }, { "n": "Weighted dip", "w": "2", "s": "3", "r": "8", "q": "8", "t": "2-3 min", "o": "Do dumbbell floor press if no access to dip handles" }, { "n": "Hanging leg raise", "w": "0", "s": "3", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Knees to chest, controlled reps, straighten legs more to increase difficulty" }, { "n": "Lat pull-over", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Can use a DB, cable/rope or band, stretch and squeeze lats!" }, { "n": "Incline dumbbell curl", "w": "1", "s": "2", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Do each arm one at a time rather than alternating, start with your weak arm" }, { "n": "Face pull", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Can use cable/rope or band, retract your shoulder blades as you pull" }] }, { "n": "Full Body 4: Deadlift, Bench Press", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "1", "r": "3", "q": "8.5", "t": "4-5 min", "o": "Work up to a heavy triple with a load that hits RPE 8-9", "c": "85-90%", "b": "Deadlift", "p": [0.85, 0.9] }, { "n": "Pause barbell bench press", "w": "3", "s": "4", "r": "6", "t": "2-3 min", "o": "2-3 second pause on the chest", "c": "75%", "b": "Bench", "p": [0.75] }, { "n": "Chest-supported T-Bar row OR Pendlay row", "w": "1", "s": "3", "r": "10", "q": "7", "t": "2-3 min", "o": "Be mindful of lower back fatigue. Stay light, minimize cheating" }, { "n": "Nordic ham curl", "w": "0", "s": "3", "r": "6-8", "q": "8", "t": "1-2 min", "o": "See video demos page in the program pdf, can sub for lying leg curl" }, { "n": "Dumbbell shrug", "w": "0", "s": "3", "r": "20-25", "q": "9", "t": "1-2 min", "o": "Feel a stretch on the traps at the bottom, squeeze hard at the top" }] }, { "n": "Full Body 5: Arm & Pump Day", "r": false, "e": [{ "n": "A1. Barbell or EZ bar curl", "w": "1", "s": "3", "r": "12", "q": "8", "t": "30sec", "o": "Curl the bar out and up in an arc. Minimize momentum." }, { "n": "A2. Floor skull crusher", "w": "1", "s": "3", "r": "12", "q": "8", "t": "30sec", "o": "Arc the bar back behind your head, soft touch on the floor behind you" }, { "n": "B1. Incline dumbbell curl (reverse 21s)", "w": "0", "s": "3", "r": "21", "q": "10", "t": "30sec", "o": "Do both arms at once: 7 reps full ROM, 7 reps top 1/2, 7 reps bottom 1/2" }, { "n": "B2. Triceps pressdown (reverse 21s)", "w": "0", "s": "3", "r": "21", "q": "10", "t": "30sec", "o": "Do both arms at once: 7 reps full ROM, 7 reps bottom 1/2, 7 reps top 1/2" }, { "n": "C1. Dumbbell lateral raise", "w": "0", "s": "3", "r": "20", "q": "9", "t": "30sec", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "C2. Band pull-apart", "w": "0", "s": "3", "r": "20", "q": "9", "t": "30sec", "o": "Mind-muscle connection with rear delts" }, { "n": "C3. Standing calf raise", "w": "0", "s": "3", "r": "12", "q": "9", "t": "30sec", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }, { "n": "C4. Bicycle crunch", "w": "0", "s": "3", "r": "15", "q": "9", "t": "30sec", "o": "Focus on rounding your back as you crunch hard!" }, { "n": "Neck flexion/extension (optional)", "w": "1", "s": "3", "r": "15/15", "q": "8", "t": "1-2 min", "o": "Avoid yanking the plate with your hands" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }], "o": [] }, { "n": "Week 8", "d": [{ "n": "Lower #1", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "3", "r": "5", "t": "3-4 min", "o": "Brace your lats, chest tall, pull the slack out of the bar before lifting", "c": "80%", "b": "Deadlift", "p": [0.8] }, { "n": "Sumo box squat or pause high-bar squat", "w": "2", "s": "2", "r": "8", "q": "7", "t": "2-3 min", "o": "If you squat high-bar, do sumo box squat. If you squat low-bar, do pause high-bar (2 sec pause)" }, { "n": "Pull-through", "w": "0", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Can use cable/rope or band, use your glutes to move the weight" }, { "n": "Leg curl", "w": "1", "s": "3", "r": "6-8", "q": "9", "t": "1-2 min", "o": "Do lying leg curl machine or nordic ham curl if no machine access" }, { "n": "Standing calf raise", "w": "1", "s": "3", "r": "8-10", "q": "9", "t": "1-2 min", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }] }, { "n": "Upper #1", "r": false, "e": [{ "n": "Barbell bench press", "w": "3", "s": "2", "r": "7", "t": "3-4 min", "o": "Set up a comfortable arch, slight pause on the chest, explode up", "c": "77.5%", "b": "Bench", "p": [0.775] }, { "n": "Chin-up", "w": "1", "s": "3", "r": "8-10", "q": "8", "t": "2-3 min", "o": "Underhand grip, pull your chest to the bar, add weight if needed to hit RPE" }, { "n": "Standing arnold dumbbell press", "w": "1", "s": "2", "r": "10-12", "q": "9", "t": "2-3 min", "o": "Rotate the DBs in at the bottom and out at the top" }, { "n": "Chest-supported dumbbell row", "w": "1", "s": "2", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Lie on an incline bench and do rows - pull with lats!" }, { "n": "Face pull", "w": "0", "s": "2", "r": "15-20", "q": "10", "t": "1-2 min", "o": "Can use cable/rope or band, retract your shoulder blades as you pull" }, { "n": "Dumbbell lateral raise", "w": "0", "s": "3", "r": "15-20", "q": "10", "t": "1-2 min", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "Concentration bicep curl", "w": "0", "s": "3", "r": "12-15", "q": "10", "t": "1-2 min", "o": "Pin your elbow against your upper leg or the back of a bench" }] }, { "n": "Lower #2", "r": false, "e": [{ "n": "Low-bar back squat", "w": "4", "s": "3", "r": "7", "t": "3-4 min", "o": "Sit back and down, keep your upper back tight to the bar", "c": "75%", "b": "Squat", "p": [0.75] }, { "n": "Good morning", "w": "2", "s": "2", "r": "10-12", "q": "7", "t": "2-3 min", "o": "Same as squat stance, keep shins straight, go lighter and \"feel\" hamstrings" }, { "n": "Leg extension", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Use bands if no machine access, mind-muscle connection with quads" }, { "n": "Standing calf raise", "w": "0", "s": "3", "r": "15-20", "q": "10", "t": "1-2 min", "o": "Emphasize the mind-muscle connection" }, { "n": "Banded lateral walk or hip abduction", "w": "0", "s": "3", "r": "15-20", "q": "10", "t": "1-2 min", "o": "Point toes slightly outward, mind-muscle connection with glutes" }, { "n": "V sit-up", "w": "0", "s": "3", "r": "12-15", "q": "10", "t": "1-2 min", "o": "Think about squeezing your upper and lower abs together" }] }, { "n": "Upper #2", "r": false, "e": [{ "n": "Overhead press / push press", "w": "3", "s": "3", "r": "3/3", "t": "3-4 min", "o": "First 3 reps strict military press (no leg drive), last 3 reps push press (use leg drive)", "c": "82.5%", "b": "OHP", "p": [0.825] }, { "n": "Single-arm lat pulldown", "w": "1", "s": "2", "r": "10-12", "q": "9", "t": "2-3 min", "o": "Perform with bands if no lat pulldown, drive elbows down and in" }, { "n": "Dumbbell incline press", "w": "2", "s": "2", "r": "12", "q": "8", "t": "2-3 min", "o": "45° incline, keep shoulder blades retracted and depressed" }, { "n": "Pendlay row", "w": "1", "s": "2", "r": "10", "q": "7", "t": "1-2 min", "o": "Be mindful of lower back fatigue. Stay light, minimize cheating" }, { "n": "Pec flye", "w": "0", "s": "3", "r": "15-20", "q": "10", "t": "1-2 min", "o": "Perform with cable, bands or dumbbells, use a full ROM" }, { "n": "A1. Incline shrug", "w": "1", "s": "2", "r": "15-20", "q": "10", "t": "30sec", "o": "Lie face down against an incline bench and do shrugs - full ROM and squeeze!" }, { "n": "A2. Bent over reverse dumbbell flye", "w": "1", "s": "2", "r": "15-20", "q": "10", "t": "30sec", "o": "Mind-muscle connection with rear delts, sweep the weight out" }, { "n": "Barbell skull crusher", "w": "1", "s": "2", "r": "8-10", "q": "10", "t": "1-2 min", "o": "Do these on a bench, constant tension on triceps" }] }, { "n": "Lower #3", "r": false, "e": [{ "n": "2\" block pull", "w": "4", "s": "2", "r": "4", "q": "8", "t": "4-5 min", "o": "Do block pulls from a 2\" block (can use 25lb bumper plate as blocks)" }, { "n": "Bulgarian split squat", "w": "1", "s": "2", "r": "12", "q": "7", "t": "2-3 min", "o": "12 reps each leg, keep your torso upright, constant-tension on quads" }, { "n": "Barbell 45° hyperextension or hip thrust", "w": "1", "s": "2", "r": "8-10", "q": "7", "t": "1-2 min", "o": "Do barbell hip thrusts if no machine, use glutes to move the weight" }, { "n": "Seated calf raise", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Do standing if no machine, emphasize the mind-muscle connection" }, { "n": "Hanging leg raise", "w": "0", "s": "3", "r": "10-12", "q": "8", "t": "1-2 min", "o": "Knees to chest, controlled reps, straighten legs more to increase difficulty" }, { "n": "Neck flexion/extension (optional)", "w": "1", "s": "3", "r": "12/12", "q": "8", "t": "1-2 min", "o": "12 reps flexion (front of neck), 12 reps extension (back of neck)" }] }, { "n": "Upper #3", "r": false, "e": [{ "n": "\"Flat-back\" barbell bench press", "w": "3", "s": "3", "r": "10", "q": "7", "t": "2-3 min", "o": "Shoulder blades still retracted and depressed. Slight arch in upper back. Minimize leg drive." }, { "n": "Eccentric-accentuated pull-up", "w": "1", "s": "2", "r": "AMRAP", "q": "10", "t": "2-3 min", "o": "3 second negative on every rep, maintain controlled form for all reps" }, { "n": "Weighted dip", "w": "2", "s": "2", "r": "10", "q": "8", "t": "2-3 min", "o": "Do dumbbell floor press if no access to dip handles" }, { "n": "Single-arm row", "w": "1", "s": "2", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Can use cables, bands or dumbbells - feel your lats working!" }, { "n": "Barbell or EZ bar curl", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Focus on the mind-muscle connection" }, { "n": "Lean-away lateral raise", "w": "0", "s": "2", "r": "30", "q": "10", "t": "1-2 min", "o": "Use a light dumbbell, constant-tension, no pause at the bottom" }, { "n": "Bicycle crunch", "w": "0", "s": "2", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Keep your hands behind your ears" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }], "o": [] }, { "n": "Week 9", "d": [{ "n": "Full Body 1: Squat, OHP", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "1", "r": "2", "q": "8.5", "t": "4-5 min", "o": "Top set. Leave 1 (maybe 2) reps in the tank. Aim for near 2 rep PR.", "c": "87.5-92.5%", "b": "Squat", "p": [0.875, 0.925] }, { "n": "Squat walk-out (do not squat)", "w": "0", "s": "1", "r": "10sec", "q": "NO REPS", "t": "4-5 min", "o": "Do not squat. Walk the weight out, hold and walk back in. Set the safety pins high and have a spotter.", "c": "100%", "b": "Squat", "p": [1.0] }, { "n": "Overhead press", "w": "2", "s": "3", "r": "6", "t": "2-3 min", "o": "Reset each rep (don't touch-and-press)", "c": "80%", "b": "OHP", "p": [0.8] }, { "n": "Glute Ham Raise", "w": "1", "s": "2", "r": "8-10", "q": "7", "t": "1-2 min", "o": "Keep your hips straight, do nordic ham curls if no GHR machine" }, { "n": "Helms row", "w": "1", "s": "2", "r": "12-15", "q": "9", "t": "1-2 mn", "o": "Strict form. Drive elbows out and back at 45 degree angle" }, { "n": "Hammer curl", "w": "0", "s": "3", "r": "20-25", "q": "10", "t": "1-2 min", "o": "Keep elbows locked in place, squeeze the dumbbell handle hard!" }] }, { "n": "Full Body 2: Deadlift, Bench Press", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "3", "r": "4", "t": "3-5 min", "o": "Semi-deload. Focus on technique and bar speed leading into max week.", "c": "80%", "b": "Deadlift", "p": [0.8] }, { "n": "Barbell bench press", "w": "4", "s": "1", "r": "2", "q": "9", "t": "4-5 min", "o": "Top set. Aim for a near 2 rep PR", "c": "87.5-92.5%", "b": "Bench", "p": [0.875, 0.925] }, { "n": "Barbell bench press", "w": "0", "s": "2", "r": "2", "t": "2-3 min", "o": "Focus on technique. Press the bar back and up with explosive force", "c": "87.5%", "b": "Bench", "p": [0.875] }, { "n": "Hip abduction", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Machine, band or weighted, 1 second isometric hold at the top of each rep" }, { "n": "Weighted pull-up", "w": "1", "s": "3", "r": "3-5", "q": "7", "t": "3-4 min", "o": "1.5x shoulder width grip, pull your chest to the bar" }, { "n": "Standing calf raise", "w": "1", "s": "3", "r": "8", "q": "9", "t": "2-3 min", "o": "1-2 second pause at the bottom of each rep, full ROM" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }, { "n": "Full Body 3: Squat, Dip", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "3", "r": "4", "t": "3-4 min", "o": "Maintain tight pressure in your upper back against the bar", "c": "82.5%", "b": "Squat", "p": [0.825] }, { "n": "Weighted dip", "w": "2", "s": "3", "r": "8", "q": "8", "t": "2-3 min", "o": "Do dumbbell floor press if no access to dip handles" }, { "n": "Hanging leg raise", "w": "0", "s": "3", "r": "10-12", "q": "9", "t": "1-2 min", "o": "Knees to chest, controlled reps, straighten legs more to increase difficulty" }, { "n": "Lat pull-over", "w": "1", "s": "3", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Can use a DB, cable/rope or band, stretch and squeeze lats!" }, { "n": "Incline dumbbell curl", "w": "1", "s": "2", "r": "12-15", "q": "9", "t": "1-2 min", "o": "Do each arm one at a time rather than alternating, start with your weak arm" }, { "n": "Face pull", "w": "0", "s": "3", "r": "15-20", "q": "9", "t": "1-2 min", "o": "Can use cable/rope or band, retract your shoulder blades as you pull" }] }, { "n": "Full Body 4: Deadlift, Bench Press", "r": false, "e": [{ "n": "Pause deadlift", "w": "4", "s": "4", "r": "2", "t": "3-4 min", "o": "3 second pause right after the plates come off the ground", "c": "75%", "b": "Deadlift", "p": [0.75] }, { "n": "Pause barbell bench press", "w": "3", "s": "3", "r": "5", "t": "2-3 min", "o": "2-3 second pause on the chest", "c": "77.5%", "b": "Bench", "p": [0.775] }, { "n": "Chest-supported T-Bar row OR Pendlay row", "w": "1", "s": "3", "r": "10", "q": "7", "t": "2-3 min", "o": "Be mindful of lower back fatigue. Stay light, minimize cheating" }, { "n": "Nordic ham curl", "w": "0", "s": "3", "r": "6-8", "q": "8", "t": "1-2 min", "o": "See video demos page in the program pdf, can sub for lying leg curl" }, { "n": "Dumbbell shrug", "w": "0", "s": "3", "r": "20-25", "q": "9", "t": "1-2 min", "o": "Feel a stretch on the traps at the bottom, squeeze hard at the top" }] }, { "n": "Full Body 5: Arm & Pump Day", "r": false, "e": [{ "n": "A1. Barbell or EZ bar curl", "w": "1", "s": "3", "r": "12", "q": "8", "t": "30sec", "o": "Curl the bar out and up in an arc. Minimize momentum." }, { "n": "A2. Floor skull crusher", "w": "1", "s": "3", "r": "10", "q": "8", "t": "30sec", "o": "Arc the bar back behind your head, soft touch on the floor behind you" }, { "n": "B1. Incline dumbbell curl (reverse 21s)", "w": "0", "s": "3", "r": "21", "q": "10", "t": "30sec", "o": "Do both arms at once: 7 reps full ROM, 7 reps top 1/2, 7 reps bottom 1/2" }, { "n": "B2. Triceps pressdown (reverse 21s)", "w": "0", "s": "3", "r": "21", "q": "10", "t": "30sec", "o": "Do both arms at once: 7 reps full ROM, 7 reps bottom 1/2, 7 reps top 1/2" }, { "n": "C1. Dumbbell lateral raise", "w": "0", "s": "3", "r": "20", "q": "9", "t": "30sec", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "C2. Band pull-apart", "w": "0", "s": "3", "r": "20", "q": "9", "t": "30sec", "o": "Mind-muscle connection with rear delts" }, { "n": "C3. Standing calf raise", "w": "0", "s": "3", "r": "12", "q": "9", "t": "30sec", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }, { "n": "C4. Bicycle crunch", "w": "0", "s": "3", "r": "15", "q": "9", "t": "30sec", "o": "Focus on rounding your back as you crunch hard!" }, { "n": "Neck flexion/extension (optional)", "w": "1", "s": "3", "r": "15/15", "q": "8", "t": "1-2 min", "o": "Avoid yanking the plate with your hands" }] }, { "n": "SUGGESTED REST DAY", "r": true, "e": [] }], "o": [] }, { "n": "Week 10A", "d": [{ "n": "Squat test", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "1", "r": "AMRAP", "q": "9.5", "t": "4-5 min", "o": "As many reps as possible. Always use a spotter and good form. Aim to hit 3+ reps", "c": "90%", "b": "Squat", "p": [0.9] }, { "n": "Single-arm lat pulldown", "w": "1", "s": "2", "r": "12", "q": "8", "t": "2-3 min", "o": "Perform with bands if no lat pulldown, drive elbows down and in" }, { "n": "Incline dumbbell curl", "w": "0", "s": "4", "r": "12", "q": "8", "t": "1-2 min", "o": "Focus on the mind-muscle connection" }, { "n": "Standing calf raise", "w": "1", "s": "3", "r": "12", "q": "8", "t": "1-2 min", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }] }, { "n": "SUGGESTED 1-2 REST DAYS", "r": true, "e": [] }, { "n": "Bench test", "r": false, "e": [{ "n": "Barbell bench press", "w": "4", "s": "1", "r": "AMRAP", "q": "9.5", "t": "4-5 min", "o": "As many reps as possible. Always use a spotter and good form. Aim to hit 3+ reps", "c": "90%", "b": "Bench", "p": [0.9] }, { "n": "Leg curl", "w": "1", "s": "3", "r": "8-10", "q": "8", "t": "2-3 min", "o": "Do lying leg curl machine or nordic ham curl if no machine access" }, { "n": "Dumbbell lateral raise", "w": "0", "s": "2", "r": "15-20", "q": "8", "t": "1-2 min", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "Triceps pressdown", "w": "1", "s": "3", "r": "12", "q": "8", "t": "1-2 min", "o": "Can do with cables or bands, squeeze triceps to move the weight" }] }, { "n": "SUGGESTED 1-2 REST DAYS", "r": true, "e": [] }, { "n": "Deadlift test", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "1", "r": "AMRAP", "q": "9.5", "t": "4-5 min", "o": "As many reps as possible. Always use a spotter and good form. Aim to hit 3+ reps", "c": "90%", "b": "Deadlift", "p": [0.9] }, { "n": "Overhead press", "w": "2", "s": "3", "r": "10", "q": "6", "t": "2-3 min", "o": "Reset each rep (don't touch-and-press)" }, { "n": "Leg extension", "w": "1", "s": "3", "r": "12", "q": "7", "t": "1-2 min", "o": "Use bands if no machine access, mind-muscle connection with quads" }, { "n": "Bicycle crunch", "w": "0", "s": "4", "r": "15", "q": "8", "t": "1-2 min", "o": "Focus on rounding your back as you crunch hard!" }] }], "o": ["IMPORTANT NOTES ABOUT WEEK 10", "If you are not feeling well recovered after completing Week 9 (achy joints, poor sleep, low energy) you should run Week 11 first and then run the Week 10 max testing. If you have accumulated sufficient fatigue from Weeks 7-9, you will likely perform better by running the deload (Week 11) first, and then running the max test week (Week 10) after.\n\n-        Always use a good spotter when attempting max effort lifts\n-        Always use safety bars on squat and bench press (in case you have to dump the bar)\n-        Do not test maxes (move to Week 11) if you are feeling joint pain\n-        Do not test maxes (move to Week 11) if you do not feel properly recovered\n-        Do not test maxes (move to Week 11) if you do not have a good spotter\n-        Maxes should be done at a 9.5 RPE: It is not necessary to push to the point where you actually fail. I recommend stopping at the point where you don’t think you could get another rep with good form.\n\n\nWhat week to run?\n\n-        Run Week 10A if you have mostly bodybuilding and strength goals\n-        Run Week 10B only if you have competitive powerlifting goals", "MAX TESTING OPTION A: Important! Choose either Week 10A or Week 10B. Do not run both weeks. See Page 88 in the program PDF for suggestions on which week to run."] }, { "n": "Week 10B", "d": [{ "n": "Squat test", "r": false, "e": [{ "n": "Back squat", "w": "5", "s": "1-3", "r": "1", "q": "9.5", "t": "4-5 min", "o": "Aim for a new PR. Start with 100% and increase by ~2.5% every attempt until you hit a 9.5 RPE. Use a spotter and good form!", "c": "100-105%", "b": "Squat", "p": [1.0, 1.05] }, { "n": "Single-arm lat pulldown", "w": "1", "s": "2", "r": "12", "q": "8", "t": "2-3 min", "o": "Perform with bands if no lat pulldown, drive elbows down and in" }, { "n": "Incline dumbbell curl", "w": "0", "s": "4", "r": "12", "q": "8", "t": "1-2 min", "o": "Focus on the mind-muscle connection" }, { "n": "Standing calf raise", "w": "1", "s": "3", "r": "12", "q": "8", "t": "1-2 min", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }] }, { "n": "SUGGESTED 1-2 REST DAYS", "r": true, "e": [] }, { "n": "Bench test", "r": false, "e": [{ "n": "Barbell bench press", "w": "5", "s": "1-3", "r": "1", "q": "9.5", "t": "4-5 min", "o": "Aim for a new PR. Start with 100% and increase by ~2.5% every attempt until you hit a 9.5 RPE. Use a spotter and good form!", "c": "100-105%", "b": "Bench", "p": [1.0, 1.05] }, { "n": "Leg curl", "w": "1", "s": "3", "r": "8-10", "q": "8", "t": "2-3 min", "o": "Do lying leg curl machine or nordic ham curl if no machine access" }, { "n": "Dumbbell lateral raise", "w": "0", "s": "2", "r": "15-20", "q": "8", "t": "1-2 min", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "Triceps pressdown", "w": "1", "s": "3", "r": "12", "q": "8", "t": "1-2 min", "o": "Can do with cables or bands, squeeze triceps to move the weight" }] }, { "n": "SUGGESTED 1-2 REST DAYS", "r": true, "e": [] }, { "n": "Deadlift test", "r": false, "e": [{ "n": "Deadlift", "w": "5", "s": "1-3", "r": "1", "q": "9.5", "t": "4-5 min", "o": "Aim for a new PR. Start with 100% and increase by ~2.5% every attempt. 5-min rest between attempts. Use good form!", "c": "100-105%", "b": "Deadlift", "p": [1.0, 1.05] }, { "n": "Overhead press", "w": "2", "s": "3", "r": "10", "q": "6", "t": "2-3 min", "o": "Reset each rep (don't touch-and-press)" }, { "n": "Leg extension", "w": "1", "s": "3", "r": "12", "q": "7", "t": "1-2 min", "o": "Use bands if no machine access, mind-muscle connection with quads" }, { "n": "Bicycle crunch", "w": "0", "s": "4", "r": "15", "q": "8", "t": "1-2 min", "o": "Focus on rounding your back as you crunch hard!" }] }], "o": ["MAX TESTING OPTION B: Important! For competitive powerlifters only. Choose either Week 10A or Week 10B. Do not run both weeks. See Page 88 in the program PDF for suggestions on which week to run."] }, { "n": "Week 11", "d": [{ "n": "Lower #1", "r": false, "e": [{ "n": "Deadlift", "w": "4", "s": "2", "r": "3", "t": "3-5 min", "o": "Brace your lats, chest tall, pull the slack out of the bar before lifting", "c": "75%", "b": "Deadlift", "p": [0.75] }, { "n": "Sumo box squat or pause high-bar squat", "w": "2", "s": "2", "r": "6", "q": "5", "t": "2-3 min", "o": "If you squat high-bar, do sumo box squat. If you squat low-bar, do pause high-bar (2 sec pause)" }, { "n": "Leg curl", "w": "1", "s": "2", "r": "6-8", "q": "6", "t": "1-2 min", "o": "Do lying leg curl machine or nordic ham curl if no machine access" }, { "n": "Standing calf raise", "w": "1", "s": "2", "r": "8-10", "q": "6", "t": "1-2 min", "o": "1-2 second pause at the bottom of each rep, full squeeze at the top" }, { "n": "Hanging leg raise", "w": "0", "s": "2", "r": "10-12", "q": "6", "t": "1-2 min", "o": "Knees to chest, controlled reps, straighten legs more to increase difficulty" }] }, { "n": "Upper #1", "r": false, "e": [{ "n": "Barbell bench press", "w": "3", "s": "3", "r": "6", "t": "3-4 min", "o": "Set up a comfortable arch, slight pause on the chest, explode up", "c": "72.5%", "b": "Bench", "p": [0.725] }, { "n": "Assisted chin-up", "w": "1", "s": "3", "r": "8-10", "q": "7", "t": "2-3 min", "o": "Underhand grip, pull your chest to the bar, add weight if needed to hit RPE" }, { "n": "Overhead press", "w": "2", "s": "2", "r": "4", "t": "2-3 min", "o": "Squeeze your glutes to keep your torso upright, press up and slightly back", "c": "75%", "b": "OHP", "p": [0.75] }, { "n": "Chest-supported dumbbell row", "w": "1", "s": "2", "r": "12-15", "q": "7", "t": "1-2 min", "o": "Lie on an incline bench and do rows - pull with lats!" }, { "n": "A1: Face pull", "w": "0", "s": "2", "r": "15-20", "q": "8", "t": "30sec", "o": "Can use cable/rope or band, retract your shoulder blades as you pull" }, { "n": "A2: Dumbbell lateral raise", "w": "0", "s": "2", "r": "15-20", "q": "8", "t": "30sec", "o": "Arc the dumbbell out, mind-muscle connection with middle fibers" }, { "n": "B1: Concentration bicep curl", "w": "0", "s": "2", "r": "12-15", "q": "8", "t": "30sec", "o": "Pin your elbow against your upper leg or the back of a bench" }, { "n": "B2: Triceps pressdown", "w": "0", "s": "2", "r": "12-15", "q": "8", "t": "30sec", "o": "Can do with cables or bands, squeeze triceps to move the weight" }] }, { "n": "SUGGESTED REST DAY (1-2 days off depending on your schedule)", "r": true, "e": [] }, { "n": "Lower #2", "r": false, "e": [{ "n": "Back squat", "w": "4", "s": "2", "r": "6", "t": "3-4 min", "o": "Sit back and down, keep your upper back tight to the bar", "c": "70%", "b": "Squat", "p": [0.7] }, { "n": "Snatch-grip Romanian deadlift", "w": "2", "s": "2", "r": "8", "q": "6", "t": "2-3 min", "o": "Wide grip, mind-muscle connection with hamstrings" }, { "n": "Leg extension", "w": "1", "s": "2", "r": "12-15", "q": "7", "t": "1-2 min", "o": "Use bands if no machine access, mind-muscle connection with quads" }, { "n": "Standing calf raise", "w": "0", "s": "3", "r": "15-20", "q": "8", "t": "1-2 min", "o": "Emphasize the mind-muscle connection" }, { "n": "Banded lateral walk or hip abduction", "w": "0", "s": "3", "r": "15-20", "q": "8", "t": "1-2 min", "o": "Point toes slightly outward, mind-muscle connection with glutes" }, { "n": "V sit-up", "w": "0", "s": "3", "r": "12-15", "q": "8", "t": "1-2 min", "o": "Think about squeezing your upper and lower abs together" }, { "n": "Neck flexion/extension (optional)", "w": "1", "s": "3", "r": "12/12", "q": "8", "t": "1-2 min", "o": "12 reps flexion (front of neck), 12 reps extension (back of neck)" }] }, { "n": "Upper #2", "r": false, "e": [{ "n": "Close-grip bench press", "w": "3", "s": "3", "r": "10", "q": "6", "t": "2-3 min", "o": "Shoulder width grip, tuck your elbows in closer to your torso" }, { "n": "Chest-supported dumbbell row", "w": "1", "s": "2", "r": "10", "q": "6", "t": "3-4 min", "o": "Lie on an incline bench and do rows - pull with lats!" }, { "n": "Weighted dip", "w": "2", "s": "3", "r": "6", "q": "7", "t": "2-3 min", "o": "Do floor dumbbell press if no access to dip handles" }, { "n": "Single-arm lat pulldown", "w": "1", "s": "2", "r": "10", "q": "8", "t": "2-3 min", "o": "Perform with bands if no lat pulldown, drive through elbows" }, { "n": "A1. Incline shrug", "w": "0", "s": "2", "r": "15-20", "q": "8", "t": "30sec", "o": "Lie against an incline bench and do shrugs - full ROM and squeeze!" }, { "n": "A2. Upright row", "w": "0", "s": "2", "r": "15-20", "q": "8", "t": "30sec", "o": "Can use cables/rope, bands or dumbbells. Stop ROM once elbows reach shoulder height." }, { "n": "B1: Barbell or EZ bar curl", "w": "0", "s": "3", "r": "12-15", "q": "8", "t": "30sec", "o": "Focus on the mind-muscle connection" }, { "n": "B2. Skull crusher", "w": "0", "s": "3", "r": "8-10", "q": "8", "t": "30sec", "o": "Barbell or EZ bar, do these on a bench, constant tension on triceps" }] }, { "n": "SUGGESTED REST DAY (1-2 days off depending on your schedule)", "r": true, "e": [] }], "o": ["Full Deload Week: Avoid failure and train lighter this week before running back through Week 1 or onto a new program."] }];
        const KEY = 'pb_planner_v1';
        const COLS = [['w', 'Warm-up sets'], ['s', 'Working sets'], ['r', 'Reps'], ['load', 'Load'], ['c', '%1RM'], ['q', 'RPE'], ['t', 'Rest'], ['o', 'Notes']];
        const SEED = { Squat: '140', Bench: '107', Deadlift: '141', OHP: '63' };
        const MAINS = [['squat', 'Squat'], ['bench', 'Bench'], ['deadlift', 'Deadlift'], ['ohp', 'OHP']];
        const ALIAS = { 'back squat': 'squat', 'low-bar back squat': 'squat', 'barbell bench press': 'bench', 'deadlift': 'deadlift', 'overhead press': 'ohp', 'overhead press / push press': 'ohp' };
        const DEF = { v: 2, unit: 'kg', comma: /^de/i.test(navigator.language || ''), cols: { w: 0, s: 1, r: 1, load: 1, c: 0, q: 1, t: 0, o: 0 }, ex: {}, fav: ['squat', 'bench', 'deadlift', 'ohp'], opt: 'A', cur: null, view: null, fold: {} };
        let S = loadS();
        function loadS() {
            let s = null; try { s = JSON.parse(localStorage.getItem(KEY) || 'null') } catch (e) { }
            const o = JSON.parse(JSON.stringify(DEF)); if (!s) return o;
            Object.assign(o, s, { cols: { ...DEF.cols, ...s.cols }, ex: { ...(s.ex || {}) }, fav: Array.isArray(s.fav) ? s.fav : DEF.fav.slice(), fold: { ...(s.fold || {}) } });
            if (s.v !== 2) {
                for (const [k, n] of MAINS) { const m = s.main && s.main[n]; if (m && m !== SEED[n] && !o.ex[k]) o.ex[k] = m }
                if (o.ex['langhantel unterarm curls'] === '22,5 x 7') delete o.ex['langhantel unterarm curls'];
                for (const a in ALIAS) { if (o.ex[a]) { const mk = ALIAS[a]; if (!o.ex[mk]) o.ex[mk] = o.ex[a]; delete o.ex[a] } }
                delete o.main; o.v = 2
            }
            return o
        }
        function save() { try { localStorage.setItem(KEY, JSON.stringify(S)) } catch (e) { } }
        const $ = id => document.getElementById(id);
        const esc = s => String(s).replace(/[&<>"]/g, c => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', '"': '&quot;' }[c]));
        const seq = () => D.filter(w => w.n !== 'Week 10' + (S.opt === 'A' ? 'B' : 'A'));
        const key = n => n.replace(/^[A-Z]\d\.\s*/, '').replace(/\s+/g, ' ').trim().toLowerCase();
        const favKey = n => { const k = key(n); return ALIAS[k] || k };
        const num = v => { const t = String(v == null ? '' : v).trim().replace(',', '.'); return /^\d+(\.\d+)?$/.test(t) ? parseFloat(t) : NaN };
        const fmt = n => { const t = String(Math.round(n * 100) / 100); return S.comma ? t.replace('.', ',') : t };
        function monday(d = new Date()) { const x = new Date(d.getFullYear(), d.getMonth(), d.getDate()); x.setDate(x.getDate() - (x.getDay() + 6) % 7); return x }
        const iso = d => d.getFullYear() + '-' + String(d.getMonth() + 1).padStart(2, '0') + '-' + String(d.getDate()).padStart(2, '0');
        function getLoad(e) {
            const fk = favKey(e.n);
            if (e.b) {
                let b = num(S.ex[fk]); if (isNaN(b)) b = num(S.ex[e.b.toLowerCase()]); if (isNaN(b)) return '–';
                const st = S.unit === 'kg' ? 2.5 : 5; return e.p.map(p => fmt(Math.round(b * p / st) * st)).join('–')
            }
            return (S.ex[fk] || '').trim();
        }
        function checkWeek() {
            if (!S.cur) return false; const s = seq(); let i = s.findIndex(w => w.n === S.cur.w); if (i < 0) i = 0;
            const diff = Math.round((monday() - new Date(S.cur.mon + 'T00:00:00')) / 604800000);
            if (diff > 0) { const n = s.length, ni = (i + diff) % n; for (let j = 1; j <= Math.min(diff, n); j++)delete S.fold[s[(i + j) % n].n]; S.cur = { w: s[ni].n, mon: iso(monday()) }; S.view = S.cur.w; save(); return true }
            return false;
        }
        function render() {
            const s = seq(); let i = s.findIndex(w => w.n === S.view); if (i < 0) i = Math.max(0, s.findIndex(w => S.cur && w.n === S.cur.w));
            const w = s[i]; S.view = w.n; const isCur = S.cur && S.cur.w === w.n;
            $('sel').innerHTML = s.map(x => `<option value="${esc(x.n)}"${x === w ? ' selected' : ''}>${esc(x.n)}${S.cur && S.cur.w === x.n ? ' ★' : ''}</option>`).join('');

            $('pos').textContent = `${i + 1} of ${s.length}${S.cur ? '' : ' · no current week set'}`;
            const m = $('mark'); m.className = isCur ? 'on' : ''; m.textContent = isCur ? '★ Current week' : '☆ Mark as current';
            const unit = S.unit === 'kg' ? 'kg' : 'lbs';
            let h = w.o.map(t => t.length > 160 ? `<details class="wn"><summary>${esc(t.slice(0, 60))}…</summary>${esc(t)}</details>` : `<div class="wn">${esc(t)}</div>`).join('');
            for (const [di, d] of w.d.entries()) {
                if (d.r) { h += `<div class="rest">${esc(d.n)}</div>`; continue }
                h += `<details class="day" data-w="${esc(w.n)}" data-d="${di}"${(S.fold[w.n] || {})[di] ? '' : ' open'}><summary>${esc(d.n)}</summary>`;
                for (const e of d.e) {
                    let c = '';
                    for (const [k, l] of COLS) {
                        if (!S.cols[k] || k === 'o') continue;
                        const v = k === 'load' ? getLoad(e) : e[k]; if (!v) continue;
                        c += `<div class="c${k === 'load' ? ' l' : ''}"><i>${k === 'load' ? unit : l.replace(' sets', '').replace('Warm-up', 'Warm')}</i><b>${esc(v)}</b></div>`;
                    }
                    const fk = favKey(e.n), fv = S.fav.includes(fk);
                    h += `<div class="ex"><div class="nm"><button class="fv${fv ? ' on' : ''}" data-fk="${esc(fk)}" aria-label="Favorite">${fv ? '★' : '☆'}</button>${esc(e.n)}</div><div class="cells">${c}</div>${S.cols.o && e.o ? `<div class="nt">${esc(e.o)}</div>` : ''}</div>`;
                }
                h += '</details>';
            }
            $('main').innerHTML = h; save();
        }
        function go(n, smooth) { S.view = n; render(); scrollTo({ top: 0, behavior: smooth ? 'smooth' : 'auto' }) }
        function step(d) { const s = seq(), n = s.length; let i = s.findIndex(w => w.n === S.view); i = ((i < 0 ? 0 : i) + d + n) % n; go(s[i].n) }
        function toast(t) { const e = document.createElement('div'); e.id = 'toast'; e.textContent = t; document.body.appendChild(e); setTimeout(() => e.remove(), 3500) }
        function tick() { if (checkWeek()) { go(S.cur.w, true); toast('New week → ' + S.cur.w) } }
        $('prev').onclick = () => step(-1); $('next').onclick = () => step(1);
        $('main').addEventListener('click', e => { const b = e.target.closest('.fv'); if (!b) return; toggleFav(b.dataset.fk); render() });
        $('sel').onchange = e => go(e.target.value);
        $('mark').onclick = () => { if (S.cur && S.cur.w === S.view) S.cur = null; else { S.cur = { w: S.view, mon: iso(monday()) }; delete S.fold[S.view] } render() };
        $('main').addEventListener('toggle', e => { const t = e.target; if (!t.classList || !t.classList.contains('day')) return; const w = t.dataset.w, d = t.dataset.d, f = S.fold[w] || {}; if (t.open) delete f[d]; else f[d] = 1; if (Object.keys(f).length) S.fold[w] = f; else delete S.fold[w]; save() }, true);
        let tx = 0, ty = 0;
        document.addEventListener('touchstart', e => { tx = e.touches[0].clientX; ty = e.touches[0].clientY }, { passive: true });
        document.addEventListener('touchend', e => { if ($('sheet').hidden === false) return; const dx = e.changedTouches[0].clientX - tx, dy = e.changedTouches[0].clientY - ty; if (Math.abs(dx) > 70 && Math.abs(dy) < 45) step(dx < 0 ? 1 : -1) }, { passive: true });
        document.addEventListener('visibilitychange', () => { if (!document.hidden) tick() });
        setInterval(tick, 60000);

        // 1RM calculator
        const MODELS = [['Epley', 'w·(1 + r/30)', (w, r) => w * (1 + r / 30)], ['Brzycki', 'w·36 / (37 − r)', (w, r) => r < 37 ? w * 36 / (37 - r) : NaN], ['Lombardi', 'w·r^0.10', (w, r) => w * Math.pow(r, 0.1)]];
        function calc() {
            const w = num($('cw').value), r = num($('cr').value), o = $('cres');
            if (isNaN(w) || isNaN(r) || w <= 0 || r < 1) { o.innerHTML = '<div class="res"><small>Enter weight and reps (1–36).</small></div>'; return }
            const v = MODELS.map(m => [m[0], m[1], r === 1 ? w : m[2](w, r)]).filter(x => isFinite(x[2]));
            const f1 = n => { const t = (Math.round(n * 10) / 10).toString(); return S.comma ? t.replace('.', ',') : t };
            o.innerHTML = v.map(x => `<div class="res"><span>${x[0]}<br><small>${x[1]}</small></span><b>${f1(x[2])} ${S.unit}</b></div>`).join('') +
                (v.length > 1 ? `<div class="res"><span>Average</span><b>${f1(v.reduce((a, x) => a + x[2], 0) / v.length)} ${S.unit}</b></div>` : '') + (r > 10 ? '<div class="res"><small>Estimates get less accurate above ~10 reps.</small></div>' : '');
        }
        $('cbtn').onclick = () => { const p = $('calc'); p.hidden = !p.hidden; $('cbtn').className = p.hidden ? '' : 'on'; if (!p.hidden) { calc(); $('cw').focus() } };
        $('cw').oninput = $('cr').oninput = calc;
        // settings
        const exMap = new Map(MAINS.map(([k, n]) => [k, { n, main: true }]));
        for (const w of D) for (const d of w.d) for (const e of d.e) { const k = favKey(e.n); if (!exMap.has(k)) exMap.set(k, { n: e.n.replace(/^[A-Z]\d\.\s*/, ''), pct: false }); const m = exMap.get(k); if (e.b && !m.main) m.pct = true }
        function toggleFav(k) { const i = S.fav.indexOf(k); if (i < 0) S.fav.push(k); else S.fav.splice(i, 1); save() }
        function srow(k, fav) { const v = exMap.get(k); return `<div class="row"><button class="fv${fav ? ' on' : ''}" data-sfk="${esc(k)}" aria-label="Favorite">${fav ? '★' : '☆'}</button><div class="g">${esc(v.n)}<small>${v.main ? 'main lift · base for % weights' : v.pct ? '% of 1RM · empty = main lift' : 'fixed weight / note'}</small></div><input type="text" data-ex="${esc(k)}" value="${esc(S.ex[k] || '')}" placeholder="–"></div>` }
        function lists() {
            const q = ($('q') ? $('q').value : '').trim().toLowerCase();
            const f = S.fav.filter(k => exMap.has(k));
            $('favl').innerHTML = f.length ? f.map(k => srow(k, true)).join('') : '<small>No favorites yet – tap ☆ on an exercise.</small>';
            $('exl').innerHTML = [...exMap].filter(([k, v]) => !S.fav.includes(k) && (!q || v.n.toLowerCase().includes(q))).sort((a, b) => a[1].n.localeCompare(b[1].n)).map(([k]) => srow(k, false)).join('');
        }
        function openSettings() {
            const sh = $('sheet');
            sh.innerHTML = `<div class="sh"><h2>Settings</h2><button id="done">Done</button></div>
      <div class="sec"><h3>General</h3>
       <div class="row"><span>Unit</span><div class="seg" data-seg="unit"><button data-v="kg" class="${S.unit === 'kg' ? 'on' : ''}">kg</button><button data-v="lbs" class="${S.unit === 'lbs' ? 'on' : ''}">lbs</button></div></div>
       <div class="row"><span>Decimal comma (112,5)</span><input type="checkbox" data-flag="comma" ${S.comma ? 'checked' : ''}></div>
       <div class="row"><span>Max-test week<small>Week 10A or 10B in the sequence</small></span><div class="seg" data-seg="opt"><button data-v="A" class="${S.opt === 'A' ? 'on' : ''}">10A</button><button data-v="B" class="${S.opt === 'B' ? 'on' : ''}">10B</button></div></div></div>
      <div class="sec"><h3>Columns</h3>${COLS.map(([k, l]) => `<div class="row"><span>${l === 'Load' ? 'Load (weight)' : l}</span><input type="checkbox" data-col="${k}" ${S.cols[k] ? 'checked' : ''}></div>`).join('')}</div>
      <div class="sec"><h3>★ Favorites (${S.unit})</h3><small style="color:var(--mu)">Main lifts are favorites by default. Enter a 1RM for %-based exercises, or a fixed weight / note otherwise (e.g. “22,5 x 7”). Tap the star to add or remove a favorite.</small><div id="favl"></div></div>
      <div class="sec"><h3>1RM / weight per exercise</h3><input id="q" type="search" placeholder="Search exercise…"><div id="exl"></div></div>
      <div class="sec"><div class="row"><span>Reset all settings</span><button id="reset">Reset</button></div></div>`;
            lists(); sh.hidden = false; document.body.style.overflow = 'hidden'; sh.scrollTop = 0;
        }
        $('gear').onclick = openSettings;
        $('sheet').addEventListener('input', e => {
            const t = e.target;
            if (t.id === 'q') { lists(); return }
            if (t.dataset.ex !== undefined) { const v = t.value.trim(); v ? S.ex[t.dataset.ex] = v : delete S.ex[t.dataset.ex] }
            else if (t.dataset.col) S.cols[t.dataset.col] = t.checked ? 1 : 0;
            else if (t.dataset.flag) S[t.dataset.flag] = t.checked;
            save()
        });
        $('sheet').addEventListener('click', e => {
            const t = e.target;
            const sf = t.closest('[data-sfk]'); if (sf) { toggleFav(sf.dataset.sfk); lists(); return }
            if (t.id === 'done') { $('sheet').hidden = true; document.body.style.overflow = ''; render(); return }
            if (t.id === 'reset') { if (confirm('Reset all settings and the current week?')) { S = JSON.parse(JSON.stringify(DEF)); save(); openSettings() } return }
            const seg = t.closest('[data-seg]'); if (seg && t.dataset.v) {
                const k = seg.dataset.seg; S[k] = t.dataset.v;
                if (k === 'opt' && S.cur && /^Week 10[AB]$/.test(S.cur.w)) S.cur.w = 'Week 10' + t.dataset.v;
                if (k === 'opt' && /^Week 10[AB]$/.test(S.view || '')) S.view = 'Week 10' + t.dataset.v;
                save(); seg.querySelectorAll('button').forEach(b => b.className = b.dataset.v === t.dataset.v ? 'on' : '')
            }
        });
        checkWeek(); render();
        if (S.cur && S.view === S.cur.w) scrollTo(0, 0);
        if ('serviceWorker' in navigator) addEventListener('load', () => navigator.serviceWorker.register('sw.js').catch(() => { }));
    </script>
</body>
</html>
