<template>
    <div class="logo-container">
        <school-logo class="mr-3"></school-logo>
        <div class="divider mr-3"></div>
        <div class="logo-text">WGG</div>
    </div>
</template>

<script setup>
import SchoolLogo from './SchoolLogo';
</script>

<style lang="scss" scoped>
.logo-container {
    display: flex;
    align-items: center;
    color: inherit;
    text-decoration: none;
    margin-right: 15px;
    .logo {
        height: 32px;
        margin-right: 12px;
    }

    .divider {
        width: 3px;
        background-color: #000;
        height: 40px;
    }

    .logo-text {
        font-size: 26px;
        font-family: 'Montserrat', sans-serif;
    }
}
</style>