<script setup>
import CharacterCard from "./CharacterCard.vue";
import { ref, onMounted, computed } from "vue";

const data = ref([]);
const loading = ref(true);
const page = ref(1)
const noMatches = ref(false);
const episodesData = ref([])

const totalPages = ref()

const pagination = computed(() => {
    const currentPage = page.value;
    const total = totalPages.value;
    const delta = 2;
    const range = [];

    for (let i = Math.max(2, currentPage - delta); i <= Math.min(total - 1, currentPage + delta); i++) {
        range.push(i);
    }

    if (currentPage - delta > 2) {
        range.unshift("...");
    }

    if (currentPage + delta < total - 1) {
        range.push("...");
    }

    range.unshift(1);
    if (total > 1) range.push(total);

    return range;
});

const changePage = (pageNumber) => {
    page.value = pageNumber;
    fetchData();
};

const selectOptions = ["Alive", "Dead", "Unknown"]
const status = ref('')
const filterInput = ref('')
const applyFilters = () => {
    fetchData()
}

const firstSeenIn = (epizode) => {
    let searchEpizode = epizode
    let firstSeenIn = episodesData.value.find(epizode => epizode.url === searchEpizode)?.name
    return firstSeenIn
};

const uniqEpisodes = ref({});

function extractEpisodes(characters) {

    const uniqueEpisodeIds = {};

    characters.forEach((character) => {
        const firstEpisodeUrl = character.episode[0];

        const episodeId = firstEpisodeUrl.match(/episode\/(\d+)/)[1];

        if (!uniqueEpisodeIds[episodeId] && !uniqEpisodes.value[episodeId]) {
            uniqueEpisodeIds[episodeId] = true;
            uniqEpisodes.value[episodeId] = true;
        }
    });
    return Object.keys(uniqueEpisodeIds);
}

const fetchData = async () => {
    try {
        noMatches.value = false
        loading.value = true;
        const response = await fetch(
            `https://rickandmortyapi.com/api/character/?page=${page.value}&name=${filterInput.value}&status=${status.value}`
        );
        const newData = await response.json();
        data.value = newData.results
        totalPages.value = newData.info.pages;
        const episodeIDs = extractEpisodes(data.value);
        if (episodeIDs.length != 0) {
            const episodesResponse = await fetch(
                `https://rickandmortyapi.com/api/episode/${episodeIDs}`
            );
            const episodesResponseData = await episodesResponse.json();
            if (Array.isArray(episodesResponseData)) {
                episodesData.value = [...episodesData.value, ...episodesResponseData];
            } else {
                episodesData.value = [...episodesData.value, episodesResponseData];
            }
        }
    } catch (error) {
        console.error(error);
        noMatches.value = true
    } finally {
        loading.value = false;
    }
};

onMounted(() => {
    fetchData();
});
</script>

<template>
    <div v-if="loading" class="loading">
        Загрузка...
    </div>
    <div v-else>
        <div class="filters">
            <input class="filters__input" v-model="filterInput" />
            <select class="filters__select" v-model="status">
                <option v-for="status in selectOptions ">{{ status }}</option>
            </select>
            <button class="filters__button" @click="applyFilters()">Применить</button>
        </div>
        <div class="charactersList">
            <div v-for="character in data" class="charactersList__item">
                <CharacterCard :name="character.name" :status="character.status" :species="character.species"
                    :location="character.location.name" :epizode="firstSeenIn(character.episode[0])"
                    :image="character.image" />
            </div>
        </div>
        <div v-if="noMatches" class="no-matches">Совпадений не найдено</div>
        <div class="pagination">
            <button v-for="pageNumber in pagination" @click="changePage(pageNumber)" :disabled="pageNumber === '...'">
                {{ pageNumber }}
            </button>
        </div>
    </div>
</template>

<style>
.loading {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.3);
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
}

.filters {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
}

.filters__input {
    padding: 8px;
    margin: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.filters__select {
    padding: 8px;
    margin: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.filters__button {
    padding: 8px 16px;
    margin: 5px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.filters__button:hover {
    background-color: #45a049;
}

.filters__button:disabled {
    background-color: #cccccc;
    color: #666666;
    cursor: not-allowed;
}

.charactersList {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    justify-content: space-evenly;
}

.no-matches {
    display: flex;
    justify-content: center;
}

.pagination {
    text-align: center;
    margin: 1rem;
}
</style>
