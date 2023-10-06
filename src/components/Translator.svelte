<script>
	import 'flowbite/dist/flowbite.css';

	
	let inputValue = '';
	let translation = '';
	let languagesFrom = 'en-US';
	let languagesTo = 'hi-IN';
	let showCloseButton = false;

	const countries = {
    "am-ET": "Amharic",
    "ar-SA": "Arabic",
    "be-BY": "Bielarus",
    "bem-ZM": "Bemba",
    "bi-VU": "Bislama",
    "bjs-BB": "Bajan",
    "bn-IN": "Bengali",
    "bo-CN": "Tibetan",
    "br-FR": "Breton",
    "bs-BA": "Bosnian",
    "ca-ES": "Catalan",
    "cop-EG": "Coptic",
    "cs-CZ": "Czech",
    "cy-GB": "Welsh",
    "da-DK": "Danish",
    "dz-BT": "Dzongkha",
    "de-DE": "German",
    "dv-MV": "Maldivian",
    "el-GR": "Greek",
    "en-US": "English",
    "es-ES": "Spanish",
    "et-EE": "Estonian",
    "eu-ES": "Basque",
    "fa-IR": "Persian",
    "fi-FI": "Finnish",
    "fn-FNG": "Fanagalo",
    "fo-FO": "Faroese",
    "fr-FR": "French",
    "gl-ES": "Galician",
    "gu-IN": "Gujarati",
    "ha-NE": "Hausa",
    "he-IL": "Hebrew",
    "hi-IN": "Hindi",
    "hr-HR": "Croatian",
    "hu-HU": "Hungarian",
    "id-ID": "Indonesian",
    "is-IS": "Icelandic",
    "it-IT": "Italian",
    "ja-JP": "Japanese",
    "kk-KZ": "Kazakh",
    "km-KM": "Khmer",
    "kn-IN": "Kannada",
    "ko-KR": "Korean",
    "ku-TR": "Kurdish",
    "ky-KG": "Kyrgyz",
    "la-VA": "Latin",
    "lo-LA": "Lao",
    "lv-LV": "Latvian",
    "men-SL": "Mende",
    "mg-MG": "Malagasy",
    "mi-NZ": "Maori",
    "ms-MY": "Malay",
    "mt-MT": "Maltese",
    "my-MM": "Burmese",
    "ne-NP": "Nepali",
    "niu-NU": "Niuean",
    "nl-NL": "Dutch",
    "no-NO": "Norwegian",
    "ny-MW": "Nyanja",
    "ur-PK": "Pakistani",
    "pau-PW": "Palauan",
    "pa-IN": "Panjabi",
    "ps-PK": "Pashto",
    "pis-SB": "Pijin",
    "pl-PL": "Polish",
    "pt-PT": "Portuguese",
    "rn-BI": "Kirundi",
    "ro-RO": "Romanian",
    "ru-RU": "Russian",
    "sg-CF": "Sango",
    "si-LK": "Sinhala",
    "sk-SK": "Slovak",
    "sm-WS": "Samoan",
    "sn-ZW": "Shona",
    "so-SO": "Somali",
    "sq-AL": "Albanian",
    "sr-RS": "Serbian",
    "sv-SE": "Swedish",
    "sw-SZ": "Swahili",
    "ta-LK": "Tamil",
    "te-IN": "Telugu",
    "tet-TL": "Tetum",
    "tg-TJ": "Tajik",
    "th-TH": "Thai",
    "ti-TI": "Tigrinya",
    "tk-TM": "Turkmen",
    "tl-PH": "Tagalog",
    "tn-BW": "Tswana",
    "to-TO": "Tongan",
    "tr-TR": "Turkish",
    "uk-UA": "Ukrainian",
    "uz-UZ": "Uzbek",
    "vi-VN": "Vietnamese",
    "wo-SN": "Wolof",
    "xh-ZA": "Xhosa",
    "yi-YD": "Yiddish",
    "zu-ZA": "Zulu"
}
  
	const translate = async () => {
	  if (!inputValue) return;
  
	  translation = 'Please Wait.....';
  
	  try {
		const response = await fetch(
		  `${main_url}${inputValue}&langpair=${languagesFrom}|${languagesTo}`
		);
		const data = await response.json();
  
		if (data.responseData && data.responseData.translatedText) {
		  translation = data.responseData.translatedText;
		} else {
		  translation = '';
		}
	  } catch (error) {
		console.error(error);
		translation = 'Error occurred';
	  }
	};
  
	const clearInput = () => {
	  inputValue = '';
	  translation = '';
	  showCloseButton = false;
	};
  
	$: showCloseButton = inputValue.length > 0;
	
	let main_url = 'https://api.mymemory.translated.net/get?q=';
	const swapLanguages = () => {
	  const tempText = inputValue;
	  inputValue = translation;
	  translation = tempText;
  
	  const tempLang = languagesTo;
	  languagesTo = languagesFrom;
	  languagesFrom = tempLang;
	};
  
	const handleLanguagesFromChange = (event) => {
	  languagesFrom = event.target.value;
	};
  
	const handleLanguagesToChange = (event) => {
	  languagesTo = event.target.value;
	};

	
	// Function to copy the translation to the clipboard
const copyTranslation = () => {
  if (translation) {
    navigator.clipboard.writeText(translation)
      .then(() => {
        // Clipboard copy success
        console.log('Translation copied to clipboard');
      })
      .catch((error) => {
        // Clipboard copy error
        console.error('Copy error:', error);
      });
  }
};

// Function to speak the translation
const speakTranslation = () => {
  if (translation) {
    const utterance = new SpeechSynthesisUtterance(translation);
	utterance.lang = languagesTo; 
    window.speechSynthesis.speak(utterance);
  }
  console.log('Speak Translation is Successful');
};

  </script>
 
  <body>
  <div class="container">
	<div class="card">
	  <div class="header">
		<h2 class="title">TRANSLATOR APP</h2>
	  </div>
	  <div class="inputs">
		<div class="first_input" id="input">
		  <select bind:value={languagesFrom} on:change={handleLanguagesFromChange}>
			{#each Object.keys(countries) as country}
			<option value={country}>{countries[country]}</option>
		{/each}
		  </select>
		  {#if showCloseButton}
			<i class="fas fa-close" id="close" on:click={clearInput}></i>
		  {/if}
		  <textarea
			id="translateFromTXTArea"
			cols="30"
			rows="10"
			placeholder="Enter Text"
			bind:value={inputValue}
			on:input={() => (showCloseButton = inputValue.length > 0)}
		  ></textarea>
		</div>
		<!-- Exchange Icon -->
		<i class="fas fa-exchange-alt" id="icon" on:click={swapLanguages} style="color: black;"></i>


<!-- Copy Icon -->
<i class="far fa-copy" id="copy" on:click={copyTranslation}></i>

<!-- Speak Icon -->
<i class="fas fa-volume-up" id="speak" on:click={speakTranslation}></i>

		<div class="second_input" id="input">
		  <select bind:value={languagesTo} on:change={handleLanguagesToChange}>
			{#each Object.keys(countries) as country}
			<option value={country}>{countries[country]}</option>
		{/each}
		  </select>
		  <textarea
			id="translateToTXTArea"
			cols="30"
			rows="10"
			placeholder="Translation"
			disabled
			bind:value={translation}
		  ></textarea>
		</div>
		<p></p>
	  </div>
	  <button class="btn" on:click={translate}>Translate</button>
	</div>
	<footer>
	  <h3>&copy; 2023 | Built By : <a href="https://github.com/kumar123-creator">Kumar Reddy</a> | All rights reserved</h3>
	</footer>
  </div>
</body>

 
<style>
	body {
	  font-family: 'Poppins', sans-serif !important;
	  background: white;
	  margin: 0;
	  padding: 0;
	  box-sizing: border-box;
	}
  
	.container {
	  position: relative;
	  min-height: 100vh;
	}
  
	.card {
	  width: 60%;
	  height: auto;
	  display: flex;
	  flex-direction: column;
	  justify-content: center;
	  position: absolute;
	  left: 50%;
	  transform: translateX(-50%);
	}
  
	.header {
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	}
  
	.title {
	  color: purple;
	  margin-block: 1%;
	  font-size: 2rem;
	}
  
	.inputs {
	  display: flex;
	}
  
	textarea {
	  width: 100%;
	  resize: none;
	  height: 50vh;
	  background: transparent;
	  font-family: inherit;
	  font-size: 1.2rem;
	  color: black;
	  padding: 1rem;
	  border-bottom-left-radius: 10px;
	  border-bottom-right-radius: 10px;
	  outline: none;
	}
  
	textarea:focus {
	  border: 1px solid rgb(170, 255, 0);
	}
  
	textarea::placeholder {
	  font-family: 'Poppins', sans-serif !important;
	  color: grey;
	  font-size: 1.2rem;
	}
  
	.first_input {
	  position: relative;
	}
  
	#close {
	  background: red;
	  width: auto;
	  padding: .4rem;
	  padding-block: .2rem;
	  z-index: 1000;
	  position: absolute;
	  right: 0;
	  cursor: pointer;
	}
  
	.hidden {
	  display: none;
	}
  
	#input select {
	  background: #2E2E2E;
	  width: 100%;
	  font-family: inherit !important;
	  border-top-left-radius: 10px;
	  border-top-right-radius: 10px;
	  color: white;
	  font-size: 1.2rem;
	}
  
	#icon {
	  font-size: 1.5rem;
	  color: white;
	  display: flex;
	  cursor: pointer;
	  margin: 1rem;
	  height: 100%;
	}
  
	.btn {
	  background: #7a583c;
	  color: white;
	  border-radius: 10px;
	  font-size: 1.5rem;
	  font-family: inherit;
	  border: none;
	  cursor: pointer;
	}
  
	.btn:hover {
	  background: #4e3b2c;
	}
  
	footer {
	  position: absolute;
	  position: fixed;
	  width: 100%;
	  bottom: 0;
	  text-align: center;
	  background: #121616;
	  padding-block: 1rem;
	  z-index: 1000;
	}
  
	footer h3 {
	  font-size: .9rem;
	  color: white;
	  font-weight: 100;
	}
  
	footer h3 a {
	  color: rgb(206, 29, 29);
	  text-decoration: none;
	  font-weight: 500;
	}
  
	footer h3 a:hover {
	  color: yellowgreen;
	}
  
	@media (max-width: 1200px) {
	  .card {
		width: 90%;
	  }
	}
  
	@media (max-width: 700px) {
	  .inputs {
		display: flex;
		flex-direction: column;
	  }
  
	  textarea {
		height: 18vh;
	  }
  
	  #icon {
		display: flex;
		align-items: center;
		justify-content: center;
		margin: .5rem;
	  }
	}
	/* Icon CSS */
.fa-exchange-alt,
.fa-copy,
.fa-volume-up {
  font-size: 1.5rem;
  color:black;
  display: flex;
  cursor: pointer;
  margin: 1rem;
  height: 100%;
}

  </style>