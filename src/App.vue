
<template>
	<div class="min-h-screen bg-gray-100">
		<div class="container py-10">
			<div class="bg-white p-4 shadow rounded">
				<div class="flex -mx-4 mb-4">

					<div class="flex-1 px-4">
						<select class="quran-input" @change="getSpecificSurah">
							<option v-for="(surah) in surahs" :value="surah.number">{{ surah.number }}. {{
								surah.englishName
							}} ({{ surah.name }}) - [{{ surah.numberOfAyahs }}]
							</option>
						</select>
					</div>

					<div class="flex-1 px-4 text-center">
						<h3 class="font-bold">{{ currentSurah.englishName }} ({{ currentSurah.name }})</h3>
						<h6 class="font-bold text-gray-500">{{ currentSurah.englishNameTranslation }}</h6>
						<p>Ayahs: <strong>{{ currentSurah.numberOfAyahs }}</strong></p>
					</div>

					<div class="flex-1 px-4 text-end">
						<select class="quran-input" name="">
							<option value="">Select Ayat</option>
						</select>
					</div>

				</div>

				<div v-if="loading" class="flex justify-center items-center">
					<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
						stroke="currentColor" class="w-16 h-16 animate-spin text-gray-300">
						<path stroke-linecap="round" stroke-linejoin="round"
							d="M10.343 3.94c.09-.542.56-.94 1.11-.94h1.093c.55 0 1.02.398 1.11.94l.149.894c.07.424.384.764.78.93.398.164.855.142 1.205-.108l.737-.527a1.125 1.125 0 011.45.12l.773.774c.39.389.44 1.002.12 1.45l-.527.737c-.25.35-.272.806-.107 1.204.165.397.505.71.93.78l.893.15c.543.09.94.56.94 1.109v1.094c0 .55-.397 1.02-.94 1.11l-.893.149c-.425.07-.765.383-.93.78-.165.398-.143.854.107 1.204l.527.738c.32.447.269 1.06-.12 1.45l-.774.773a1.125 1.125 0 01-1.449.12l-.738-.527c-.35-.25-.806-.272-1.203-.107-.397.165-.71.505-.781.929l-.149.894c-.09.542-.56.94-1.11.94h-1.094c-.55 0-1.019-.398-1.11-.94l-.148-.894c-.071-.424-.384-.764-.781-.93-.398-.164-.854-.142-1.204.108l-.738.527c-.447.32-1.06.269-1.45-.12l-.773-.774a1.125 1.125 0 01-.12-1.45l.527-.737c.25-.35.273-.806.108-1.204-.165-.397-.505-.71-.93-.78l-.894-.15c-.542-.09-.94-.56-.94-1.109v-1.094c0-.55.398-1.02.94-1.11l.894-.149c.424-.07.765-.383.93-.78.165-.398.143-.854-.107-1.204l-.527-.738a1.125 1.125 0 01.12-1.45l.773-.773a1.125 1.125 0 011.45-.12l.737.527c.35.25.807.272 1.204.107.397-.165.71-.505.78-.929l.15-.894z" />
						<path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
					</svg>
					<span class="font-bold ml-2 text-gray-300 animate-pulse">Loading...</span>
				</div>

				<div v-else="loading" class="ayahs-wrapper border-t">
					<div class="single-ayah" v-if="currentSurah.hasOwnProperty('ayahs')"
						v-for="ayah in currentSurah.ayahs">
						<span class="ayah-number">
							{{ ayah.numberInSurah }}
						</span>
						<span>
							{{ ayah.text }}
						</span>
					</div>
				</div>

			</div>
		</div>
	</div>
</template>

<script>
import axios from 'axios';

export default {
	name: "App",
	data() {
		return {
			surahs: [],
			currentSurah: [],
			loading: true
		}
	},
	mounted() {
		axios.get('https://api.alquran.cloud/v1/surah')
			.then(response => {
				this.surahs = response.data.data
			})
		this.quarySpacificSurah(1)
	},
	methods: {
		getSpecificSurah(e) {
			this.quarySpacificSurah(e.target.value)
		},
		quarySpacificSurah(surahNumber) {
			this.loading = true;
			axios.get('https://api.alquran.cloud/v1/surah/' + surahNumber)
				.then(response => {
					this.loading = true;
					this.currentSurah = response.data.data;
					this.loading = false;

					console.log(response.data.data)
				});
		}
	}
}
</script>
			
<!-- axios.get('https://api.alquran.cloud/v1/surah/' + surahNumber + '/editions/quran-uthmani,en.asad,bn.bengali') -->