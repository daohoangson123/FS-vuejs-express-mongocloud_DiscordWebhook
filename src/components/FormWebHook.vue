<template>
    <form
        @submit="handleSubmit"
        class="grid gap-2 p-5 mx-auto max-w-[450px] h-[480px] border-l border-gray-600 rounded">
        <div class="flex justify-between">
            <span class="text-indigo-950 text-lg font-semibold">
                Add New Intergration
            </span>
            <span>X</span>
        </div>
        <div class="type grid gap-1">
            <label
                for="type"
                class="font-semibold"
                >Type</label
            >
            <div class="select relative">
                <select
                    v-bind:class="[type === 'Discord' && 'pl-8']"
                    name=""
                    class="w-full h-10 border-[1px] border-gray-500 rounded font-semibold cursor-pointer appearance-none bg-no-repeat bg-[center_right_10px] bg-[length:10px_10px] bg-[url('data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%23131313%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E')]"
                    id="type"
                    v-model="type"
                    required>
                    <option value="">Select Type</option>
                    <option value="Discord">Discord</option>
                </select>
                <img
                    class="absolute top-[10px] left-2 w-5 h-5"
                    src="../assets/discord-icon.svg"
                    alt=""
                    v-if="type === 'Discord'" />
            </div>
        </div>
        <div class="friendly grid gap-1">
            <label
                for="name"
                class="font-semibold"
                >Friendly Name <span class="text-red-600">*</span></label
            >
            <input
                class="border-[1px] h-10 border-gray-500 rounded pl-2"
                type="text"
                id="name"
                required
                v-model="name" />
        </div>
        <div class="hook grid gap-1">
            <label
                for="hook"
                class="font-semibold"
                >Webhook <span class="text-red-600">*</span></label
            >
            <input
                class="border-[1px] h-10 border-gray-500 rounded pl-2"
                type="text"
                id="hook"
                required
                v-model="hook" />
        </div>
        <div class="bot grid gap-1">
            <label
                for="bot"
                class="font-semibold"
                >Bot Display Name</label
            >
            <input
                class="border-[1px] h-10 border-gray-500 rounded pl-2"
                type="text"
                id="bot"
                v-model="bot"
                placeholder="BWatch" />
        </div>
        <div class="flex gap-3">
            <button
                id="test"
                class="rounded h-8 text-slate-400 font-semibold bg-slate-200 w-[50%] hover:bg-slate-600 transition-all"
                @click="handleAction">
                Send Test
            </button>
            <button
                id="save"
                class="rounded h-8 text-white font-semibold bg-slate-400 w-[50%] hover:bg-slate-700 transition-all"
                @click="handleAction">
                Save
            </button>
        </div>
    </form>
</template>
<script>
import axios from 'axios';

export default {
    data() {
        return {
            type: 'Discord',
            name: 'My Discord Alert (1)',
            hook: '',
            bot: '',
        };
    },
    name: 'FormWebHook',
    methods: {
        handleSubmit(e) {
            e.preventDefault();
        },
        handleAction(e) {
            const action = e.target.id;

            const embeds = [
                {
                    title: this.name || 'Lorem ipsum',
                    footer: {
                        text: this.bot || 'Lorem ipsum',
                    },
                    fields: [
                        {
                            name: this.bot || 'Lorem ipsum',
                            value: this.name || 'Lorem ipsum',
                        },
                    ],
                },
            ];

            let data = JSON.stringify({ embeds });

            const config = {
                method: 'POST',
                url: this.hook,
                headers: { 'Content-Type': 'application/json' },
                data: data,
            };

            const fakeconfig = {
                method: 'POST',
                url: this.hook,
                headers: { 'Content-Type': 'application/json' },
                data: '',
            };

            if (action === 'test') {
                this.sendMess(config);
            } else if (action === 'save') {
                this.sendMess(fakeconfig);
            }
        },
        async sendMess(config) {
            if (
                this.type === 'Discord' &&
                this.name !== '' &&
                this.hook !== ''
            ) {
                axios(config)
                    .then((response) => {
                        window.alert('Webhook delivered successfully');
                        console.log(response);
                        return 123;
                    })
                    .catch((error) => {
                        if (error.request.status === 400) {
                            this.saveData();
                        }
                        if (
                            error.request.status === 404 ||
                            error.code === 'ERR_NETWORK'
                        ) {
                            window.alert('Webhook URL is invalid');
                        }
                        console.log(error);
                    });
            }
        },
        async saveData() {
            const formData = {
                type: this.type,
                name: this.name,
                hook: this.hook,
                bot: this.bot,
            };
            if (
                this.type === 'Discord' &&
                this.name !== '' &&
                this.hook !== ''
            ) {
                axios
                    .post(
                        'http://localhost:5050/api/Discord/savedata',
                        formData,
                    )
                    .then((response) => {
                        return response;
                    })
                    .catch((error) => {
                        window.alert(
                            'Oops! Something wrong, please try again later',
                        );
                        return error;
                    });
                window.alert('Data had been save to MongoDB Cloud');
            }
        },
    },
};
</script>
