<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>English Vocabulary Game</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #6a93cb 0%, #a4bfef 100%);
            min-height: 100vh;
            padding: 20px;
            color: #2c3e50;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            max-width: 1100px;
            width: 100%;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
        }

        .header h1 {
            font-size: 2.2em;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 10px;
            padding: 20px;
            background: #f8f9fa;
            flex-wrap: wrap;
        }

        .nav-btn {
            padding: 12px 20px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 15px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(74, 111, 165, 0.4);
        }

        .nav-btn:not(.active) {
            background: white;
            color: #666;
        }

        .nav-btn:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-1px);
        }

        .game-section {
            display: none;
            padding: 30px;
            min-height: 500px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #e8f4f2);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            border: 2px solid #4a6fa5;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .word-bank h3 {
            color: #4a6fa5;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.4em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 14px;
            box-shadow: 0 3px 10px rgba(74, 111, 165, 0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(74, 111, 165, 0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 20px;
            border-left: 5px solid #4a6fa5;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }

        .question h3 {
            color: #4a6fa5;
            margin-bottom: 15px;
            font-size: 1.2em;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 8px;
            padding: 8px 12px;
            font-size: 16px;
            margin: 0 5px;
            min-width: 120px;
            transition: border-color 0.3s ease;
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 12px;
            margin-top: 15px;
        }

        .option {
            padding: 12px 18px;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .option:hover {
            border-color: #4a6fa5;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .option.selected {
            background: #4a6fa5;
            color: white;
            border-color: #4a6fa5;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 25px;
            margin-top: 20px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 15px;
            color: #4a6fa5;
            font-size: 1.2em;
        }

        .match-item {
            padding: 12px;
            margin: 6px 0;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
            font-size: 14px;
        }

        .match-item:hover {
            border-color: #4a6fa5;
            transform: translateY(-1px);
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .match-item.selected {
            background: #e8f4f2;
            border-color: #4a6fa5;
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
        }

        .check-btn {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 20px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.3);
        }

        .check-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(74, 111, 165, 0.4);
        }

        .reset-btn {
            background: linear-gradient(135deg, #f44336, #ff7961);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 10px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(244, 67, 54, 0.3);
        }

        .reset-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(244, 67, 54, 0.4);
        }

        .feedback {
            margin-top: 15px;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 15px 20px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.3);
            z-index: 1000;
        }

        .icon {
            font-size: 24px;
            margin-right: 10px;
            vertical-align: middle;
        }

        /* Vocabulary List Styles */
        .vocabulary-list {
            padding: 20px;
        }

        .vocabulary-item {
            margin-bottom: 20px;
            padding: 18px;
            border-radius: 10px;
            background: #f8f9fa;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
        }

        .vocabulary-item h3 {
            color: #4a6fa5;
            margin-bottom: 8px;
            font-size: 1.3em;
            border-bottom: 2px solid #4a6fa5;
            padding-bottom: 6px;
        }

        .vocabulary-item p {
            margin-bottom: 6px;
            line-height: 1.5;
        }

        .example {
            font-style: italic;
            color: #555;
            padding-left: 15px;
            border-left: 3px solid #4a6fa5;
            margin-top: 8px;
        }

        .uk-flag {
            display: inline-block;
            width: 20px;
            height: 15px;
            background: linear-gradient(to bottom, #012169 33%, #FFFFFF 33%, #FFFFFF 66%, #C8102E 66%);
            margin-right: 8px;
            border-radius: 2px;
            vertical-align: middle;
        }

        .conversation-example {
            background: #e8f4f8;
            padding: 12px;
            border-radius: 8px;
            margin-top: 8px;
            font-size: 0.9em;
        }

        .conversation-example p {
            margin: 4px 0;
            font-style: italic;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 20px;
            }
            
            .nav-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .nav-btn {
                width: 200px;
                margin-bottom: 5px;
            }
            
            .score {
                position: static;
                margin: 20px auto;
                display: block;
                width: fit-content;
            }
            
            .word-option {
                font-size: 12px;
                padding: 6px 12px;
            }
        }
    </style>
</head>
<body>
    <div class="score">Score: <span id="score">0</span>/12</div>
    
    <div class="container">
        <div class="header">
            <h1><i class="fas fa-book icon"></i> English Vocabulary Game</h1>
            <p>Learn and practice these English words and phrases!</p>
        </div>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showSection('vocabulary')">Vocabulary List</button>
            <button class="nav-btn" onclick="showSection('fill-gaps')">Fill in the Gaps</button>
            <button class="nav-btn" onclick="showSection('matching')">Match Definitions</button>
            <button class="nav-btn" onclick="showSection('multiple-choice')">Multiple Choice</button>
        </div>

        <!-- Vocabulary List Section -->
        <div id="vocabulary" class="game-section active">
            <h2 style="text-align: center; margin-bottom: 20px; color: #4a6fa5;">English Vocabulary</h2>
            
            <div class="vocabulary-list">
                <div class="vocabulary-item">
                    <h3><span class="uk-flag"></span>glue</h3>
                    <p><strong>Definition:</strong> A sticky substance used for joining things together.</p>
                    <p><strong>Part of Speech:</strong> noun/verb</p>
                    <p class="example"><strong>Example:</strong> "We need some glue to fix this broken vase."</p>
                    <div class="conversation-example">
                        <p><strong>Conversation:</strong></p>
                        <p>"How do I attach these pieces?"</p>
                        <p>"Use glue - it will hold them together."</p>
                    </div>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="uk-flag"></span>trouble</h3>
                    <p><strong>Definition:</strong> Problems or difficulties; a situation that causes problems.</p>
                    <p><strong>Part of Speech:</strong> noun</p>
                    <p class="example"><strong>Example:</strong> "I'm having trouble understanding this lesson."</p>
                    <div class="conversation-example">
                        <p><strong>Conversation:</strong></p>
                        <p>"Are you having trouble with the homework?"</p>
                        <p>"Yes, I don't understand question three."</p>
                    </div>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="uk-flag"></span>boring</h3>
                    <p><strong>Definition:</strong> Not interesting or exciting; dull.</p>
                    <p><strong>Part of Speech:</strong> adjective</p>
                    <p class="example"><strong>Example:</strong> "The movie was so boring that I fell asleep."</p>
                    <div class="conversation-example">
                        <p><strong>Conversation:</strong></p>
                        <p>"How was the lecture?"</p>
                        <p>"It was boring - the speaker just read from his notes."</p>
                    </div>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="uk-flag"></span>without</h3>
                    <p><strong>Definition:</strong> Not having or using something; lacking.</p>
                    <p><strong>Part of Speech:</strong> preposition</p>
                    <p class="example"><strong>Example:</strong> "I can't make coffee without milk."</p>
                    <div class="conversation-example">
                        <p><strong>Conversation:</strong></p>
                        <p>"Can you help me with this?"</p>
                        <p>"I can't do it without the instructions."</p>
                    </div>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="uk-flag"></span>prank</h3>
                    <p><strong>Definition:</strong> A trick or practical joke played on someone.</p>
                    <p><strong>Part of Speech:</strong> noun</p>
                    <p class="example"><strong>Example:</strong> "My brother played a prank on me by hiding my phone."</p>
                    <div class="conversation-example">
                        <p><strong>Conversation:</strong></p>
                        <p>"Why is there tape on the door?"</p>
                        <p>"It's just a prank - someone is playing jokes today."</p>
                    </div>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="uk-flag"></span>strong</h3>
                    <p><strong>Definition:</strong> Having physical power; not easily broken or damaged.</p>
                    <p><strong>Part of Speech:</strong> adjective</p>
                    <p class="example"><strong>Example:</strong> "You need to be strong to lift that heavy box."</p>
                    <div class="conversation-example">
                        <p><strong>Conversation:</strong></p>
                        <p>"This coffee is very strong!"</p>
                        <p>"Yes, I made it with extra coffee grounds."</p>
                    </div>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="uk-flag"></span>study</h3>
                    <p><strong>Definition:</strong> To spend time learning about a subject; the activity of learning.</p>
                    <p><strong>Part of Speech:</strong> verb/noun</p>
                    <p class="example"><strong>Example:</strong> "I need to study for my English exam tomorrow."</p>
                    <div class="conversation-example">
                        <p><strong>Conversation:</strong></p>
                        <p>"What are you doing tonight?"</p>
                        <p>"I have to study for my test."</p>
                    </div>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="uk-flag"></span>french</h3>
                    <p><strong>Definition:</strong> Relating to France, its people, or its language.</p>
                    <p><strong>Part of Speech:</strong> adjective/noun</p>
                    <p class="example"><strong>Example:</strong> "I'm learning to speak French."</p>
                    <div class="conversation-example">
                        <p><strong>Conversation:</strong></p>
                        <p>"What languages do you speak?"</p>
                        <p>"I speak English and a little French."</p>
                    </div>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="uk-flag"></span>buy</h3>
                    <p><strong>Definition:</strong> To get something by paying money for it.</p>
                    <p><strong>Part of Speech:</strong> verb</p>
                    <p class="example"><strong>Example:</strong> "I want to buy a new phone."</p>
                    <div class="conversation-example">
                        <p><strong>Conversation:</strong></p>
                        <p>"Where did you buy those shoes?"</p>
                        <p>"I bought them at the mall yesterday."</p>
                    </div>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="uk-flag"></span>wealthy</h3>
                    <p><strong>Definition:</strong> Having a lot of money, possessions, or resources.</p>
                    <p><strong>Part of Speech:</strong> adjective</p>
                    <p class="example"><strong>Example:</strong> "The wealthy businessman donated money to the hospital."</p>
                    <div class="conversation-example">
                        <p><strong>Conversation:</strong></p>
                        <p>"He lives in a very big house."</p>
                        <p>"Yes, his family is quite wealthy."</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- Fill in the Gaps Section -->
        <div id="fill-gaps" class="game-section">
            <div class="word-bank">
                <h3><i class="fas fa-list-ul icon"></i> Word Bank - Choose from these words:</h3>
                <div class="word-options">
                    <span class="word-option">glue</span>
                    <span class="word-option">trouble</span>
                    <span class="word-option">boring</span>
                    <span class="word-option">without</span>
                    <span class="word-option">prank</span>
                    <span class="word-option">strong</span>
                    <span class="word-option">study</span>
                    <span class="word-option">french</span>
                    <span class="word-option">buy</span>
                    <span class="word-option">wealthy</span>
                </div>
            </div>

            <div id="fill-gaps-container">
                <!-- Sentences will be dynamically inserted here -->
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Check Answers</button>
            <button class="reset-btn" onclick="resetFillBlanks()">Reset Answers</button>
        </div>

        <!-- Matching Section -->
        <div id="matching" class="game-section">
            <h2 style="text-align: center; margin-bottom: 20px; color: #4a6fa5;">Match the words with their definitions</h2>
            <div class="matching-container">
                <div class="match-column">
                    <h4>Vocabulary Words</h4>
                    <div class="match-item" data-word="glue" onclick="selectMatch(this)">glue</div>
                    <div class="match-item" data-word="trouble" onclick="selectMatch(this)">trouble</div>
                    <div class="match-item" data-word="boring" onclick="selectMatch(this)">boring</div>
                    <div class="match-item" data-word="without" onclick="selectMatch(this)">without</div>
                    <div class="match-item" data-word="prank" onclick="selectMatch(this)">prank</div>
                    <div class="match-item" data-word="strong" onclick="selectMatch(this)">strong</div>
                    <div class="match-item" data-word="study" onclick="selectMatch(this)">study</div>
                    <div class="match-item" data-word="french" onclick="selectMatch(this)">french</div>
                    <div class="match-item" data-word="buy" onclick="selectMatch(this)">buy</div>
                    <div class="match-item" data-word="wealthy" onclick="selectMatch(this)">wealthy</div>
                </div>
                <div class="match-column">
                    <h4>Definitions</h4>
                    <div id="meanings-container">
                        <!-- Meanings will be dynamically inserted here -->
                    </div>
                </div>
            </div>
            <div class="feedback" id="matching-feedback" style="display: none;"></div>
            <button class="check-btn" onclick="checkMatching()">Check Matching</button>
            <button class="reset-btn" onclick="resetMatching()">Reset Matching</button>
        </div>

        <!-- Multiple Choice Section -->
        <div id="multiple-choice" class="game-section">
            <div class="question">
                <h3>Question 1: What is "glue" used for?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Cleaning surfaces</div>
                    <div class="option" onclick="selectOption(this, true)">Joining things together</div>
                    <div class="option" onclick="selectOption(this, false)">Painting walls</div>
                    <div class="option" onclick="selectOption(this, false)">Writing on paper</div>
                </div>
                <div class="feedback" id="mc-feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 2: If you have "trouble" with something, you:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Find it very easy</div>
                    <div class="option" onclick="selectOption(this, true)">Have problems or difficulties</div>
                    <div class="option" onclick="selectOption(this, false)">Don't care about it</div>
                    <div class="option" onclick="selectOption(this, false)">Finished it quickly</div>
                </div>
                <div class="feedback" id="mc-feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 3: Something that is "boring" is:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Very exciting</div>
                    <div class="option" onclick="selectOption(this, true)">Not interesting or exciting</div>
                    <div class="option" onclick="selectOption(this, false)">Very funny</div>
                    <div class="option" onclick="selectOption(this, false)">Very important</div>
                </div>
                <div class="feedback" id="mc-feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 4: "Without" means:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">With a lot of something</div>
                    <div class="option" onclick="selectOption(this, true)">Not having or using something</div>
                    <div class="option" onclick="selectOption(this, false)">Next to something</div>
                    <div class="option" onclick="selectOption(this, false)">Inside something</div>
                </div>
                <div class="feedback" id="mc-feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 5: A "prank" is:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">A serious conversation</div>
                    <div class="option" onclick="selectOption(this, true)">A trick or practical joke</div>
                    <div class="option" onclick="selectOption(this, false)">A type of food</div>
                    <div class="option" onclick="selectOption(this, false)">A study method</div>
                </div>
                <div class="feedback" id="mc-feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 6: If something is "strong", it is:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Easy to break</div>
                    <div class="option" onclick="selectOption(this, true)">Not easily broken or damaged</div>
                    <div class="option" onclick="selectOption(this, false)">Very light</div>
                    <div class="option" onclick="selectOption(this, false)">Very expensive</div>
                </div>
                <div class="feedback" id="mc-feedback-6" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Question 7: To "study" means to:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Play games</div>
                    <div class="option" onclick="selectOption(this, true)">Spend time learning about a subject</div>
                    <div class="option" onclick="selectOption(this, false)">Watch television</div>
                    <div class="option" onclick="selectOption(this, false)">Go shopping</div>
                </div>
                <div class="feedback" id="mc-feedback-7" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Question 8: "French" refers to:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">A type of food only</div>
                    <div class="option" onclick="selectOption(this, true)">The language, people, or culture of France</div>
                    <div class="option" onclick="selectOption(this, false)">A city in England</div>
                    <div class="option" onclick="selectOption(this, false)">A type of car</div>
                </div>
                <div class="feedback" id="mc-feedback-8" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Question 9: To "buy" something means to:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Sell it to someone</div>
                    <div class="option" onclick="selectOption(this, true)">Get it by paying money</div>
                    <div class="option" onclick="selectOption(this, false)">Borrow it from a friend</div>
                    <div class="option" onclick="selectOption(this, false)">Make it yourself</div>
                </div>
                <div class="feedback" id="mc-feedback-9" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Question 10: A "wealthy" person has:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Very little money</div>
                    <div class="option" onclick="selectOption(this, true)">A lot of money and possessions</div>
                    <div class="option" onclick="selectOption(this, false)">Many problems</div>
                    <div class="option" onclick="selectOption(this, false)">A large family</div>
                </div>
                <div class="feedback" id="mc-feedback-10" style="display: none;"></div>
            </div>
            
            <button class="reset-btn" onclick="resetMultipleChoice()">Reset Answers</button>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];
        
        // Track correct answers for each section
        let fillBlanksCorrect = 0;
        let matchingCorrect = 0;
        let multipleChoiceCorrect = 0;
        
        // Definitions for the matching game
        const definitions = [
            { meaning: "glue", text: "A sticky substance used for joining things" },
            { meaning: "trouble", text: "Problems or difficulties" },
            { meaning: "boring", text: "Not interesting or exciting; dull" },
            { meaning: "without", text: "Not having or using something" },
            { meaning: "prank", text: "A trick or practical joke" },
            { meaning: "strong", text: "Having physical power; not easily broken" },
            { meaning: "study", text: "To spend time learning about a subject" },
            { meaning: "french", text: "Relating to France, its people, or language" },
            { meaning: "buy", text: "To get something by paying money" },
            { meaning: "wealthy", text: "Having a lot of money and possessions" }
        ];

        // Sentences for the fill-in-the-blanks game
        const sentences = [
            { text: "We need some <input type='text' class='fill-blank' data-answer='glue' placeholder='answer'> to fix this broken vase.", answer: "glue" },
            { text: "I'm having <input type='text' class='fill-blank' data-answer='trouble' placeholder='answer'> understanding this lesson.", answer: "trouble" },
            { text: "The movie was so <input type='text' class='fill-blank' data-answer='boring' placeholder='answer'> that I fell asleep.", answer: "boring" },
            { text: "I can't make coffee <input type='text' class='fill-blank' data-answer='without' placeholder='answer'> milk.", answer: "without" },
            { text: "My brother played a <input type='text' class='fill-blank' data-answer='prank' placeholder='answer'> on me by hiding my phone.", answer: "prank" },
            { text: "You need to be <input type='text' class='fill-blank' data-answer='strong' placeholder='answer'> to lift that heavy box.", answer: "strong" },
            { text: "I need to <input type='text' class='fill-blank' data-answer='study' placeholder='answer'> for my English exam tomorrow.", answer: "study" },
            { text: "I'm learning to speak <input type='text' class='fill-blank' data-answer='French' placeholder='answer'>.", answer: "French" },
            { text: "I want to <input type='text' class='fill-blank' data-answer='buy' placeholder='answer'> a new phone.", answer: "buy" },
            { text: "The <input type='text' class='fill-blank' data-answer='wealthy' placeholder='answer'> businessman donated money to the hospital.", answer: "wealthy" },
            { text: "This box is very <input type='text' class='fill-blank' data-answer='strong' placeholder='answer'> - it won't break easily.", answer: "strong" },
            { text: "Can you help me <input type='text' class='fill-blank' data-answer='buy' placeholder='answer'> a birthday present for my mother?", answer: "buy" }
        ];

        // Function to shuffle array (Fisher-Yates algorithm)
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // Initialize the fill-in-the-blanks game with shuffled sentences
        function initFillBlanks() {
            const container = document.getElementById('fill-gaps-container');
            container.innerHTML = '';
            
            // Shuffle the sentences
            const shuffledSentences = shuffleArray([...sentences]);
            
            // Create and append the sentence elements
            shuffledSentences.forEach((sentence, index) => {
                const div = document.createElement('div');
                div.className = 'question';
                div.innerHTML = `
                    <h3>Question ${index + 1}:</h3>
                    <p>${sentence.text}</p>
                    <div class="feedback" id="feedback-${index + 1}" style="display: none;"></div>
                `;
                container.appendChild(div);
            });
        }

        // Initialize the matching game with shuffled definitions
        function initMatchingGame() {
            const meaningsContainer = document.getElementById('meanings-container');
            meaningsContainer.innerHTML = '';
            
            // Shuffle the definitions
            const shuffledDefinitions = shuffleArray([...definitions]);
            
            // Create and append the definition elements
            shuffledDefinitions.forEach(def => {
                const div = document.createElement('div');
                div.className = 'match-item';
                div.setAttribute('data-meaning', def.meaning);
                div.onclick = function() { selectMatch(this); };
                div.textContent = def.text;
                meaningsContainer.appendChild(div);
            });
        }

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.add('active');
            
            // Add active class to clicked button
            event.target.classList.add('active');
            
            // If showing fill-in-the-blanks section, reinitialize with shuffled sentences
            if (sectionId === 'fill-gaps') {
                initFillBlanks();
            }
            
            // If showing matching section, reinitialize with shuffled definitions
            if (sectionId === 'matching') {
                initMatchingGame();
                // Reset matching game state
                document.querySelectorAll('.match-item').forEach(item => {
                    item.classList.remove('selected', 'matched');
                });
                selectedWord = null;
                selectedMeaning = null;
                matchedPairs = [];
                document.getElementById('matching-feedback').style.display = 'none';
            }
            
            // Update score display
            updateScore();
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank, index) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
                
                // Show feedback for each question
                const feedback = document.getElementById(`feedback-${index+1}`);
                if (userAnswer === correctAnswer) {
                    feedback.textContent = '✅ Correct!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Incorrect. The correct answer is: "${blank.dataset.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            });
            
            fillBlanksCorrect = correctCount;
            updateScore();
        }

        function resetFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            blanks.forEach(blank => {
                blank.value = '';
                blank.classList.remove('correct', 'incorrect');
            });
            
            const feedbacks = document.querySelectorAll('#fill-gaps .feedback');
            feedbacks.forEach(feedback => {
                feedback.style.display = 'none';
            });
            
            fillBlanksCorrect = 0;
            updateScore();
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('matching-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Correct match!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === definitions.length) {
                    feedback.textContent = '🎉 Congratulations! You matched all terms correctly!';
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Incorrect match. Try again.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
            
            matchingCorrect = matchedPairs.length;
            updateScore();
        }

        function checkMatching() {
            const allMatches = document.querySelectorAll('.match-item[data-word]');
            let allCorrect = true;
            
            allMatches.forEach(item => {
                if (!item.classList.contains('matched')) {
                    allCorrect = false;
                }
            });
            
            const feedback = document.getElementById('matching-feedback');
            if (allCorrect) {
                feedback.textContent = '🎉 Excellent! All matches are correct!';
                feedback.className = 'feedback correct';
            } else {
                const unmatchedCount = definitions.length - matchedPairs.length;
                feedback.textContent = `You have ${unmatchedCount} unmatched pair${unmatchedCount !== 1 ? 's' : ''}. Keep trying!`;
                feedback.className = 'feedback incorrect';
            }
            feedback.style.display = 'block';
        }

        function resetMatching() {
            document.querySelectorAll('.match-item').forEach(item => {
                item.classList.remove('selected', 'matched');
            });
            
            initMatchingGame();
            
            selectedWord = null;
            selectedMeaning = null;
            matchedPairs = [];
            
            document.getElementById('matching-feedback').style.display = 'none';
            
            matchingCorrect = 0;
            updateScore();
        }

        function selectOption(element, isCorrect) {
            // Remove selection from all options in this question
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
                opt.classList.remove('correct');
                opt.classList.remove('incorrect');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            const questionNumber = element.closest('.question').querySelector('h3').textContent.match(/\d+/)[0];
            const feedback = document.getElementById(`mc-feedback-${questionNumber}`);
            
            if (isCorrect) {
                element.classList.add('correct');
                feedback.textContent = '✅ Correct!';
                feedback.className = 'feedback correct';
            } else {
                element.classList.add('incorrect');
                feedback.textContent = '❌ Incorrect. Try again.';
                feedback.className = 'feedback incorrect';
                
                // Show the correct answer
                options.forEach(opt => {
                    if (opt.onclick.toString().includes('true')) {
                        opt.classList.add('correct');
                    }
                });
            }
            
            feedback.style.display = 'block';
            
            // Count correct answers in multiple choice
            multipleChoiceCorrect = document.querySelectorAll('#multiple-choice .option.correct.selected').length;
            updateScore();
        }

        function resetMultipleChoice() {
            document.querySelectorAll('#multiple-choice .option').forEach(option => {
                option.classList.remove('selected', 'correct', 'incorrect');
            });
            
            document.querySelectorAll('#multiple-choice .feedback').forEach(feedback => {
                feedback.style.display = 'none';
            });
            
            multipleChoiceCorrect = 0;
            updateScore();
        }

        function updateScore() {
            // Total score
            score = fillBlanksCorrect + matchingCorrect + multipleChoiceCorrect;
            document.getElementById('score').textContent = score;
        }
        
        // Initialize the page
        window.onload = function() {
            initFillBlanks();
            initMatchingGame();
        }
    </script>
</body>
</html>
