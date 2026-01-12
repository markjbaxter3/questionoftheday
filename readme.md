<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daily Conversation Starter</title>
    <style>
        /* Modern, Clean Look */
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: #f0f2f5;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            color: #333;
            padding: 20px;
            box-sizing: border-box;
        }

        .container {
            text-align: center;
            width: 100%;
            max-width: 500px;
        }

        .card {
            background: white;
            padding: 40px 30px;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            position: relative;
            transition: transform 0.3s ease;
            display: flex;
            flex-direction: column;
            justify-content: center;
            min-height: 300px;
        }

        .card:hover {
            transform: translateY(-5px);
        }

        .label {
            text-transform: uppercase;
            letter-spacing: 2px;
            font-size: 0.75rem;
            color: #888;
            margin-bottom: 25px;
            display: block;
            font-weight: 700;
        }

        .question {
            font-size: 1.4rem;
            line-height: 1.4;
            font-weight: 600;
            color: #2c3e50;
            margin-bottom: 40px;
            flex-grow: 1;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .button-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
            align-items: center;
        }

        .btn {
            background-color: #007aff;
            color: white;
            border: none;
            padding: 14px 28px;
            border-radius: 50px;
            font-size: 1rem;
            cursor: pointer;
            text-decoration: none;
            width: 100%;
            max-width: 250px;
            transition: background 0.2s, transform 0.1s;
            font-weight: 500;
        }

        .btn:hover {
            background-color: #0056b3;
        }

        .btn:active {
            transform: scale(0.98);
        }

        .btn-secondary {
            background-color: transparent;
            color: #666;
            border: 2px solid #e1e4e8;
        }
        
        .btn-secondary:hover {
            background-color: #f8f9fa;
            border-color: #d1d5da;
        }
        
        .footer {
            margin-top: 25px;
            font-size: 0.75rem;
            color: #aaa;
        }
    </style>
</head>
<body>

<div class="container">
    <div class="card">
        <span class="label" id="date-label">Today's Question</span>
        <div class="question" id="daily-question">
            Loading question...
        </div>
        
        <div class="button-group">
            <button class="btn" onclick="copyText()">Copy to Text</button>
            <button class="btn btn-secondary" onclick="randomQuestion()">Reshuffle</button>
        </div>
    </div>
    <div class="footer">Refreshes automatically at midnight</div>
</div>

<script>
    // --- FULL LIST OF 200+ QUESTIONS ---
    const questions = [
        "If you took a photo of me at my absolute happiest, what would it be?",
        "When do you feel the most 'yourself'?",
        "What is one worry you are carrying right now that you haven't told me about?",
        "If you could relive one single day from your childhood, which day would it be?",
        "What is the one lesson you learned from your parents that you want to pass on?",
        "When do you feel most appreciated by me?",
        "What is a small, mundane moment we’ve shared that you think about often?",
        "If you could describe our marriage using only one word, what would it be?",
        "What is one thing I do that drives you crazy, but you secretly find endearing?",
        "In what specific area do you think we have grown the most as a couple?",
        "If you were to brag about me to a stranger, what is the first thing you would say?",
        "If we could drop everything and move to a different country for one year, where?",
        "If you had to listen to one album for the rest of your life, what would it be?",
        "If we won the lottery today, what is the first 'irresponsible' purchase you would make?",
        "What is the one goal you haven't achieved yet that you are afraid you might run out of time for?",
        "Ten years from now, on a perfect Sunday morning, what are we doing?",
        "What is a skill or hobby you have always wanted to pick up?",
        "What's the best compliment you've ever received?",
        "What is your favorite memory of us dating?",
        "If you could change one decision from your past, what would it be?",
        "What is one thing you’ve forgiven me for that I probably don't even realize?",
        "If you could call your 20-year-old self for one minute, what would you say?",
        "What is a compliment you received years ago that stuck with you?",
        "What is the hardest truth you've ever had to accept about yourself?",
        "When you are feeling overwhelmed, what is the one specific thing I can do that actually helps?",
        "Is there a dream you've let go of that you still think about sometimes?",
        "What is a tradition we’ve started that you hope we keep forever?",
        "If we were characters in a book, would we be the comic relief, the heroes, or the villains?",
        "What is the most attractive thing I do when I don't know you're watching?",
        "If you lost your memory, what is the first story I should tell you to help you remember who we are?",
        "What is one thing we do better than any other couple we know?",
        "What is your favorite 'hard time' that we survived together?",
        "If we had to start a business together tomorrow, what would it be?",
        "If you could have dinner with any three people from history, who would make the cut?",
        "What is the weirdest thing you find attractive?",
        "If we had to be on a reality TV show, which one would we win?",
        "If you could instantly download one skill into your brain (Matrix style), what would it be?",
        "What song always reminds you of me, even if the lyrics don't make sense?",
        "What is the most valuable thing we own that isn't money or a physical object?",
        "What is one rule we have for our family that you think makes us different from everyone else?",
        "When we are 80 years old, sitting on a porch, what do you think we will laugh about most?",
        "If we could give our family one superpower, what would it be?",
        "What is a place you haven't been to yet that you feel a 'pull' to visit?",
        "What do you hope is the first thing people say about you when you leave the room?",
        "What is the one thing you are most proud of achieving in your life so far?",
        "What is a fear you have overcome that you are proud of?",
        "When was the last time you cried tears of joy?",
        "What is the biggest risk you have ever taken, and was it worth it?",
        "If you could see a measuring stick above people's heads, what would you want it to measure?",
        "What is the most painful thing someone has ever said to you?",
        "What is something you believe now that you didn't believe 5 years ago?",
        "Do you think you are more like your mom or your dad?",
        "What is a habit you have that you wish you could break?",
        "If you knew you would die in exactly one year, what would you change about how you are living now?",
        "What is the one thing you want to be remembered for?",
        "What does a 'perfect day' look like to you from waking up to going to sleep?",
        "What is something you feel like you are 'late' to in life?",
        "If you could erase one bad memory from your mind, would you do it?",
        "What is the most spiritual experience you have ever had?",
        "What makes you feel most insecure?",
        "What makes you feel most confident?",
        "What was your very first impression of me?",
        "At what exact moment did you realize you were in love with me?",
        "What is your favorite physical feature of mine?",
        "What is your favorite non-physical trait of mine?",
        "If we could relive our wedding day, would you change anything?",
        "What is the best date we have ever been on?",
        "What is one thing I do that makes you feel safe?",
        "What is one household chore you secretly don't mind doing?",
        "What is one household chore you absolutely despise?",
        "If you could change one thing about my communication style, what would it be?",
        "How do you think we handle conflict compared to other couples?",
        "What is a song that comes on the radio that makes you think of a specific time in our relationship?",
        "What is the funniest thing we have ever argued about?",
        "If I could do one thing to make your daily life 10% easier, what would it be?",
        "What is the most romantic thing I have ever done for you?",
        "What is the most thoughtful gift I have ever given you?",
        "Do you think we spend enough quality time together?",
        "What is one new activity you would like to try together this year?",
        "How do you think we have changed each other for the better?",
        "What is something I am passionate about that you don't fully understand but support anyway?",
        "If we were to renew our vows, what is one new promise you would add?",
        "What is your favorite photo of us?",
        "What is one inside joke we have that no one else would understand?",
        "Who is the 'strict one' and who is the 'fun one' in our relationship?",
        "What is the biggest compromise you have made for me?",
        "What is the biggest compromise I have made for you?",
        "If you were a professional wrestler, what would your entrance theme song be?",
        "If animals could talk, which one would be the rudest?",
        "If you could shrink any animal to the size of a cat and keep it as a pet, what would it be?",
        "If you had to eat one meal for every meal for the rest of your life, what would it be?",
        "If you could have a secret tunnel in our house that led anywhere, where would it go?",
        "If you were a ghost, who would you haunt and how?",
        "If you were famous, what would you be famous for?",
        "If you could master one musical instrument overnight, which one would it be?",
        "If you could live in any TV show universe, which one would it be?",
        "What is the worst movie you have ever seen?",
        "What is your controversial food opinion?",
        "If you could ban one word from the dictionary, what would it be?",
        "If you could un-invent one piece of technology, what would it be?",
        "If you were arrested with no explanation, what would your friends assume you did?",
        "What would the title of your autobiography be?",
        "If you could have an unlimited supply of one thing (besides money), what would it be?",
        "If you had to survive a zombie apocalypse, what would be your weapon of choice?",
        "Which fictional family is most like ours?",
        "If you could teleport, where is the first place you would go right now?",
        "If you could choose your own nickname, what would it be?",
        "What is the hardest part about being a parent?",
        "What is the most rewarding part about being a parent?",
        "What is one rule your parents had that you swore you’d never enforce, but now you do?",
        "What is one thing you hope our kids say about us when they are adults?",
        "If our family had a motto or crest, what would be on it?",
        "What is the most important holiday tradition for you?",
        "What is one thing you want to do with our family before the kids leave the house?",
        "How are we different from your parents?",
        "How are we similar to your parents?",
        "What is the best piece of parenting advice you’ve ever received?",
        "What is a childhood toy you wish you still had?",
        "What was your favorite subject in school?",
        "Who was your favorite teacher and why?",
        "What is one thing you wish you had learned in school but didn't?",
        "If you could give our children only one piece of advice for their lives, what would it be?",
        "What is the funniest thing our kids have ever done?",
        "What is your proudest moment as a parent so far?",
        "Do you think we are too strict, too lenient, or just right?",
        "What is one family meal you want to pass down?",
        "What is the most expensive mistake you made as a child?",
        "Sunrise or Sunset?",
        "Coffee or Tea?",
        "Beach vacation or Mountain vacation?",
        "Fancy dinner out or Takeout on the couch?",
        "Comedy or Thriller movie?",
        "Physical book or Kindle?",
        "Dogs or Cats?",
        "Summer or Winter?",
        "Sweet or Savory?",
        "Driver or Passenger?",
        "Planner or Spontaneous?",
        "Shower or Bath?",
        "Text or Call?",
        "City life or Country life?",
        "Hotel or Airbnb?",
        "Card games or Board games?",
        "Fiction or Non-fiction?",
        "Museum or Amusement Park?",
        "Beer or Wine?",
        "Cooking or Cleaning?",
        "What is your favorite time of day to be intimate?",
        "What is one fantasy you have that we haven't tried yet?",
        "What outfit of mine do you find the sexiest?",
        "What is the biggest turn-on for you?",
        "What is the biggest turn-off for you?",
        "Do you prefer slow and romantic or fast and intense?",
        "What is your favorite spot in our house to make love (besides the bed)?",
        "How does our intimacy now compare to when we first met?",
        "What is something I do outside of the bedroom that turns you on?",
        "If we had a 'date night' with no budget and no kids, what would we do?",
        "What is the sexiest scene from a movie you remember?",
        "How important is physical touch to you on a daily basis?",
        "What is your favorite way to be kissed?",
        "Do you like public displays of affection?",
        "What is the best massage I've ever given you?",
        "If you didn't need money, what job would you do?",
        "What was your very first job?",
        "What is the worst job you ever had?",
        "What is your favorite thing about your current career?",
        "What is the most stressful part of your week?",
        "Do you prefer working alone or in a team?",
        "If you could start a business, what would it be?",
        "What is a career path you almost took but didn't?",
        "What is the best career advice you've ever gotten?",
        "Do you work to live or live to work?",
        "What is your biggest professional accomplishment?",
        "If you could retire tomorrow, how would you fill your days?",
        "What is a skill you use at work that I probably don't know about?",
        "Who is your favorite coworker and why?",
        "How do you handle stress at work?",
        "Do you believe in aliens?",
        "Do you believe in ghosts?",
        "What is your favorite smell?",
        "What is the worst haircut you ever had?",
        "What is the best concert you have ever been to?",
        "What is a song you know every word to?",
        "What is your 'guilty pleasure' TV show?",
        "If you could be an Olympic athlete, what sport would you play?",
        "What is your favorite season and why?",
        "What is your favorite ice cream flavor?",
        "What is the strangest dream you’ve ever had?",
        "If you could change your first name, what would you change it to?",
        "What is something you are terrible at but enjoy doing?",
        "What is your favorite board game?",
        "What is the longest you’ve ever gone without sleep?",
        "Have you ever broken a bone?",
        "What is your favorite quote?",
        "If you could design your dream house, what is one must-have feature?",
        "What is the most courageous thing you have ever done?",
        "What is the kindest thing a stranger has ever done for you?",
        "If you could speak any language fluently, which one would it be?",
        "What is your favorite vegetable?",
        "What is your least favorite vegetable?",
        "What is your favorite way to exercise?",
        "If you could meet any celebrity, who would it be?",
        "What is the best book you have read in the last year?",
        "What is your favorite thing to do on a rainy day?",
        "If you were a color, which one would you be?",
        "What is the most beautiful place you have ever seen in person?",
        "If you could go back to school, what would you study?",
        "What is your favorite sound?",
        "What is your least favorite sound?",
        "Do you think technology has made life better or worse?",
        "What is a movie you can watch over and over again?",
        "What is the best piece of advice you would give your younger self about love?",
        "What is one thing you want to accomplish in the next month?",
        "What is one thing you want to accomplish in the next year?",
        "What is your favorite thing about yourself?",
        "What is your favorite thing about our life together?"
    ];

    // Logic to pick a question based on the Day of the Year
    function getDailyQuestion() {
        const now = new Date();
        const start = new Date(now.getFullYear(), 0, 0);
        const diff = now - start;
        const oneDay = 1000 * 60 * 60 * 24;
        const dayOfYear = Math.floor(diff / oneDay);
        
        // Use modulo operator to cycle through the array endlessly
        const index = dayOfYear % questions.length;
        
        document.getElementById('daily-question').innerText = questions[index];
        
        // Set Date Label
        const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
        document.getElementById('date-label').innerText = now.toLocaleDateString('en-US', options);
    }

    // Function to manually pick a random one
    function randomQuestion() {
        // Simple animation effect
        const qContainer = document.getElementById('daily-question');
        qContainer.style.opacity = 0;
        
        setTimeout(() => {
            const randomIndex = Math.floor(Math.random() * questions.length);
            qContainer.innerText = questions[randomIndex];
            document.getElementById('date-label').innerText = "Random Pick";
            qContainer.style.opacity = 1;
        }, 200);
    }

    // Copy to clipboard function
    function copyText() {
        const text = document.getElementById('daily-question').innerText;
        navigator.clipboard.writeText(text).then(() => {
            // Visual feedback on button
            const btn = document.querySelector('.btn');
            const originalText = btn.innerText;
            btn.innerText = "Copied!";
            btn.style.backgroundColor = "#28a745"; // Green color
            
            setTimeout(() => {
                btn.innerText = originalText;
                btn.style.backgroundColor = "#007aff"; // Back to blue
            }, 1500);
        });
    }

    // Initialize
    getDailyQuestion();

</script>

</body>
</html>
