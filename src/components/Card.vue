<template>
    <div :class="cardClassName">
        <div :class="cardClassNameHead">
            <img :src="card.avatar_url" :alt="card.author" class="avatar" />
            <div class="card-center">
                <span class="colorname">{{ card.author }}</span>
                <span>posted on {{ formattedDate }}</span>
            </div>
        </div>
        <!-- <hr class="divider" /> -->
        <div class="card-body">
            <img :src="card.image_url" :alt="card.title" class="card-image" @click="toggleModal" />
            <!-- <div class="image-container">
                <div v-if="imageLoading" class="loader">Loading...</div>
                <img v-if="!imageLoading" :src="card.image_url" :alt="card.title" class="card-image" @load="onImageLoad"
                    @error="onImageError" @click="toggleModal" />
            </div> -->
            <div class="card-content">
                <h2>{{ card.title }}</h2>
                <p>{{ card.body }}</p>
            </div>
        </div>
        <div v-if="showModal" class="modal" @click="toggleModal">
            <div class="modal-content">
                <img :src="card.image_url" :alt="card.title" />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        card: Object
    },
    data() {
        return {
            showModal: false,
            imageLoading: true
        };
    },
    computed: {
        cardClassName() {
            return this.card.id % 2 === 0 ? "card even" : "card";
        },
        cardClassNameHead(){
            return this.card.id % 2 === 0 ? "card-header even" : "card-header";
        },  
        formattedDate() {
            const options = {
                weekday: 'long',
                year: 'numeric',
                month: 'long',
                day: 'numeric',
                hour: '2-digit',
                minute: '2-digit',
                hour12: false,
                timeZone: 'Asia/Bangkok'
            };
            return new Date(this.card.created_at).toLocaleDateString('en-US', options).replace(", ", ", ");
        }
    },
    methods: {
        toggleModal() {
            this.showModal = !this.showModal;
        },
        onImageLoad() {
            this.imageLoading = false;
        },
        onImageError() {
            this.imageLoading = false;
        }
    }
};
</script>

<style scoped>
.card {
    background-color: #FFF;
    border: 1px solid #ddd;
    overflow: hidden;
    margin: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    width: 80%;
}

.even {
    background-color: #ccecff;
}

.card-header.even{
    background-color: #ccecff; 
}

.card-header {
    padding: 10px;
    font-size: 0.9em;
    color: #999;
    background-color: #FFF;
    border-bottom: 1px solid #ddd;
    display: flex;
}

.card-header .avatar {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    margin-right: 10px;
}

.card-center {
    display: flex;
    align-items: center;
    font-weight: 600;
}

.colorname {
    color: #FF8C00;
    margin-right: 5px;
}

.divider {
    margin: 0;
    border: 0;
    border-top: 1px solid #ddd;
}

.card-body {
    padding: 10px;
    display: flex;
    align-items: flex-start;
}

.image-container {
    position: relative;
    width: 150px;
    height: 150px;
}

.card-image {
    width: 150px;
    height: auto;
    cursor: pointer;
}

.loader {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 14px;
    color: #888;
}

.card-content {
    text-align: left;
    padding-left: 10px;
    padding-right: 10px;
    flex: 1;
}

.card-content h2 {
    margin: 0 0 5px;
    font-size: 1.1em;
}

.card-content p {
    margin: 0 0 10px;
    font-size: 1em;
    color: #555;
}

.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal-content {
    max-width: 90%;
    max-height: 90%;
    overflow: auto;
}

.modal-content img {
    width: 720px;
    height: auto;
}
</style>