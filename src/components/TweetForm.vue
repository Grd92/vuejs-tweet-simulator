<template>
    <div class="tweet-form container" :class="{ open: showForm }">
        <form @submit.prevent="sendTweet">
            <input
                class="form-control"
                placeholder="Tu nombre"
                type="text"
                v-model="username"
            />

            <textarea
                rows="3"
                class="form-control"
                placeholder="Escribe tu tweet"
                v-model="tweet"
            ></textarea>

            <button type="submnit" class="btn btn-success">Enviar</button>
        </form>
    </div>
</template>

<script>
import { ref } from "vue";
import { saveTweetApi } from "../api/tweet";

export default {
    props: {
        showForm: Boolean,
        openCloseForm: Function,
        reloadTweets: Function,
    },
    setup(props) {
        let username = ref("");
        let tweet = ref("");

        const sendTweet = () => {
            if (!tweet.value || !username.value) return;

            saveTweetApi(tweet.value, username.value);

            username.value = "";
            tweet.value = "";

            props.openCloseForm();
            props.reloadTweets();
        };

        return {
            sendTweet,
            username,
            tweet,
        };
    },
};
</script>

<style lang="scss" scoped>
.tweet-form {
    margin-top: 20px;
    height: 0;
    overflow: hidden;

    &.open {
        height: auto;
    }

    form {
        margin-bottom: 50px;

        input {
            margin-bottom: 10px;
        }
        button {
            margin-top: 10px;
            width: 100%;
        }
    }
}
</style>
