<script lang="ts">

import { Microphone, Magnify, AccountCircleOutline, DotsVertical, Menu, Close } from 'mdue';
import YTIcon from './icons/Logo.vue';
export default {
    data() {
        return {
            focusSrch: false,
        }
    },
    components: {
        YTIcon,
        Microphone, Magnify, AccountCircleOutline, DotsVertical, Menu, Close
    },
    methods: {
        searchInput: function(event) {
            if (event.target.value != "") 
                this.$refs.removesearchbtn.classList.remove("hidden")
            else this.$refs.removesearchbtn.classList.add("hidden")
        },
        clearSearch: function(event) {
            this.$refs.searchinput.value = ""
            document.querySelector('.removeSearch')?.classList.add("hidden") 
        },
        updateBorder: function() {
            if (!this.focusSrch) {
                this.$refs.srchfrm.style.border = "1px solid #1c62b9";
                this.$refs.srchfrm.style.marginLeft = "0";
                this.$refs.searchinput.style.marginLeft = "9.5px"
                this.focusSrch = true
                this.$refs.srchicon.style.display = "inline-flex"
                this.$refs.srchicon.style.position = "relative"
            } else {
                this.$refs.srchfrm.style.border = "1px solid hsl(0, 0%, 18.82%)";
                this.$refs.srchfrm.style.marginLeft = "32px";
                this.focusSrch = false
                this.$refs.srchicon.style.display = "none"
                this.$refs.srchicon.style.position = "absolute"
                this.$refs.searchinput.style.marginLeft = "0"
            }
        }
    }
}
</script>

<template>
    <header class="header">
        <div class="header-container">
            <div class="start">
                <div class="hamburger-menu">
                    <Menu></Menu>
                </div>
                <div class="logo">
                    <RouterLink to="/">
                        <YTIcon fill="white" display="flex" />
                    </RouterLink>
                </div>
            </div>
            <div class="center">
                <div class="search">
                    <div class="container" ref="srchcont">
                        <form class="searchForm" ref="srchfrm">
                            <div class="searchIcon" ref="srchicon">
                                <magnify></magnify>
                            </div>
                            <input type="text" placeholder="Search" @input="searchInput" ref="searchinput" @focus="updateBorder" @focusout="updateBorder">
                            <div class="removeSearch hidden" @click="clearSearch" ref="removesearchbtn">
                                <close></close>
                            </div>
                        </form>
                        <button type="submit"><magnify></magnify></button>
                    </div>
                    <button class="voiceSearch"><microphone></microphone></button>
                </div>
            </div>
            <div class="end">
                <div class="settings">
                    <dots-vertical></dots-vertical>
                </div>
                <div class="login">
                    <button class="login-btn">
                        <account-circle-outline></account-circle-outline>
                        <span>SIGN IN</span>
                    </button>
                </div>
            </div>
        </div>
    </header>
</template>

<style lang="scss">
.hidden {
    visibility: hidden;
}

.header {
    width: 100%;
    height: 56px;
    &-container {
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: space-between;
        align-items: center;
        text-align: center;
        padding: 0 18px 0 6px;
        .start {
            display: flex;
            align-items: center;
            text-align: center;
            height: 56px;

            div { margin-left: 12px; }

            .hamburger-menu {
                color: white;
                padding: 8px;
                border-radius: 999px;
                font-size: 24px;
                &:hover {
                    cursor: pointer;
                    background: #212121;
                }

                svg {
                    display: flex;
                    width: 100%;
                    height: 100%;
                }
            }
            .logo {
                width: 96px;
                height: 80px;
            }
        }

        .center {
            height: 56px;
            align-items: center;
            text-align: center;
            flex: 0 1 728px;
            .search {
                display: flexbox;
                display: flex;
                flex-direction: row;
                align-items: center;
                height: 56px;
                min-width: 0px;
                width: 100%;
                
                .container {
                    display: flex;
                    margin: 0 0 0 40px;
                    width: 100%;
                    padding: 0 4px;
                    flex: 1;
                    flex-basis: 1e-9px;
                    form {
                        width: 100%;
                        border: 1px solid hsl(0, 0%, 18.82%);
                        border-right: none;
                        overflow: hidden;
                        border-radius: 40px 0 0 40px;
                        position: relative;
                        display: flex;
                        background: hsl(0, 0%, 7%);
                        padding: 0px 4px 0px 16px;
                        margin-left: 32px;
                        input {
                            color: white;
                            background: transparent;
                            width: 100%;
                            height: 100%;
                            display: flex;
                            font-size: 16px;
                            border: 0;
                            font-weight: 500;
                            border: 0;
                            outline: none;
                        }
                        .searchIcon {
                            pointer-events: none;
                            position: absolute;
                            top: 0;
                            right: 0;
                            bottom: 0;
                            left: 0;
                            width: 24px;
                            font-size: 24px;
                            color: white;
                            display: none;
                            height: 100%;
                            align-items: center;
                            svg {
                                display: flex;
                                width: 100%;
                                height: 100%;
                            }
                        }
                        .removeSearch {
                            position: absolute;
                            z-index: 5000;
                            color: white;
                            transform: translate(-50%, -50%);
                            left: 95%;
                            top: 50%;
                            width: 40px;
                            height: 40px;
                            align-items: center;
                            text-align: center;
                            font-size: 24px;
                            display: flex;
                            border-radius: 96px;
                            z-index: 90;
                            &:hover {
                                cursor: pointer;
                                background: #212121;
                            }

                            svg {
                                display: flex;
                                width: 100%;
                            }
                        }
                    }
                    button {
                        color: white;
                        background-color: hsla(0, 0%, 100%, 0.08);
                        border: 1px solid hsl(0, 0%, 18.82%);
                        border-radius: 0 40px 40px 0;
                        cursor: pointer;
                        height: 40px;
                        max-width: 64px;
                        min-width: 64px;
                        margin: 0;
                        font-size: 24px;
                        svg{
                            display: flex;
                            width: 100%;
                        }
                    }
                }
                .voiceSearch {
                    border-radius: 80px;
                    border: 0;
                    width: 40px;
                    height: 40px;
                    max-width: 40px;
                    max-height: 40px;
                    min-width: 40px;
                    min-height: 40px;
                    background: #181818;
                    color: white;
                    text-align: center;
                    align-items: center;
                    justify-content: center;
                    font-size: 24px;
                    margin-left: 10px;
                    svg {
                        display: flex;
                        width: 100%;
                    }
                    &:hover {
                        cursor: pointer;
                        background: #212121;
                    }
                }
            }
        }
        .end {
            height: 56px;
            display: flex;
            align-items: center;
            text-align: center;
            justify-content: space-between;
            .settings {
                color: white;
                margin-top: 4px;
                &:hover {
                    cursor: pointer;
                }
                svg {
                    height: 100%;
                    font-size: 24px;
                }
            }
            .login {
                &-btn {
                    display: flex;
                    height: 36px;
                    background: none;
                    border: 0;
                    color: #438ae0;
                    align-items: center;
                    font-weight: 500;
                    text-align: center;
                    width: 94px;
                    justify-content: center;
                    border: 1px solid hsl(0, 0%, 25%);
                    border-radius: 999px;
                    font-size: 21px;
                    span {
                        font-size: 14px;
                        margin-left: 2px;
                    }
                    &:hover {
                        background: #438ae040;
                        border: 1px solid #438ae040;
                        cursor: pointer;
                    }
                }
            }
        }
    }
}
</style>