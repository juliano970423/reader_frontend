<template>
    <div class="menu">
        <a-menu mode="horizontal" theme="light" :selected-keys="[currentSelectedKey]">
            <a-menu-item key="title" :style="{ padding: 0, marginRight: '38px' }" disabled>
                <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTC_FfAd7ExirZN83GGBMdWUbqXRhR7wlmafA&s" alt="logo" style=" width: 80px;" />
            </a-menu-item>
            <a-menu-item key="/" @click="navigateTo('/')"><icon-home />首頁</a-menu-item>
            <a-menu-item key="/articles" @click="navigateTo('/articles')"><icon-book />文章</a-menu-item>
            <!---<a-menu-item key="/UploadView" @click="navigateTo('/UploadView')"><icon-upload />上傳文章</a-menu-item>--->
            <a-sub-menu style="background-color:rgba(255, 255, 255, 0);" key="/0">
                <template #icon><icon-apps></icon-apps></template>
                <template #title>小工具</template>
                <a-menu-item key="/snake" @click="navigateTo('/calculator')">計算機</a-menu-item>
            </a-sub-menu>
        </a-menu>
        <div class="menu-right" align="right">
            <!--
            <div :style="{ padding: '20px' }">
                <a-trigger trigger="click" position="left" auto-fit-position :unmount-on-close="false">
                    <icon-search />
                    <template #content>
                        <div class="trigger-search">
                            <div align="center">
                                <a-input-search :style="{ width: '320px' }" align="right"
                                    placeholder="Please enter something" />
                            </div>
                        </div>
                    </template>
                </a-trigger>
            </div>
            -->
            <div :style="{ padding: '20px' }">
                <a-trigger trigger="click" position="top" auto-fit-position :unmount-on-close="false">
                    <IconUser />
                    <template #content>
                        <div class="trigger-user">
                            <div align="center" :style="{ padding: '10px' }">
                                <a-avatar :style="{ backgroundColor: '#3370ff', marginBottom: '10px' }">
                                    <IconUser />
                                </a-avatar>
                                <br />
                                <label>{{ getUsername }}</label>
                                <br />
                                <a-button type="primary" size="small" :style="{ marginTop: '10px' }" href="/login"
                                    v-if="!isLoggedIn">登入</a-button>
                                <a-button type="primary" size="small" :style="{ marginTop: '10px' }" @click="handleLogout()"
                                    v-else-if="isLoggedIn">登出</a-button>
                            </div>
                        </div>
                    </template>
                </a-trigger>
            </div>
        </div>
    </div>
</template>
<style scoped>
.menu {
    box-sizing: border-box;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    height: 60px;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    width: 100%;
    padding: 0px;
    background-color: var(--color-bg-2);
}

.menu-right {
    display: flex;
    padding: 0px;
}

.trigger-search {
    padding: 10px;
    width: 350px;
    background-color: var(--color-bg-popup);
    border-radius: 4px;
    box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.15);
}

.trigger-user {
    padding: 10px;
    width: 250px;
    background-color: var(--color-bg-popup);
    border-radius: 4px;
    box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.15);
}
</style>
<script>
import { useAuthStore } from '@/stores/auth.js';
import { computed } from 'vue';
export default {
    name: 'NavBar',
    setup() {
        const authStore = useAuthStore();// 使用 computed 来定义 isLoggedIn
        const isLoggedIn = computed(() => authStore.getLoginStatus);

        // 使用 computed 来定义 getUsername
        const getUsername = computed(() => {
            const username = authStore.currentUser;
            return username ? username : "username";
        });
        const handleLogout = () => {
            console.log("logout");
            authStore.logout();
        }
        return {
            isLoggedIn,
            getUsername,
            handleLogout,
        }
    },
    computed: {
        currentSelectedKey() {
            return this.$route.path.startsWith('/articles') ? '/articles' : this.$route.path;
        }
    },
    methods: {
        navigateTo(path) {
            this.$router.push(path); // 使用 Vue Router 的 push 方法实现导航
        },
    },
};
</script>