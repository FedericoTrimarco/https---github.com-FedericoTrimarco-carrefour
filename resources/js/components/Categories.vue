<template>
    <section class="categories" v-if="categories !== null">
        <div class="selector left">
            <button class="btn btn-primary" @click="scrollRight">
                <i class="fas fa-angle-left"></i>
            </button>
        </div>

        <ul id="drag" ref="list">
            <Category 
                v-for="category in categories" 
                :key="`category-${category.id}`" 
                :categoryData="category"
            />
        </ul>

        <div class="selector right">
            <button class="btn right btn-primary" @click="scrollLeft">
                <i class="fas fa-angle-right"></i>
            </button>
        </div>
    </section>
</template>

<script>
import axios from 'axios';

// gsap
import gsap from "gsap";
import { Draggable } from "gsap/Draggable";
gsap.registerPlugin(Draggable);

// Components import
import Category from './Category';

// BaseURL for axios api call
const baseURL = 'http://127.0.0.1:8000'; // da modificare in caso di discrepanza

export default {
    name: 'Categories',
    components: {
        Category,
    },
    data() {
        return {
            categories: null,
            translateX: 0,
        }
    },
    // computed: {
    //     ul() {
    //         return document.querySelector('.categories ul');
    //     },
    // },
    created() {
        this.getCategories();
    },
    methods: {
        getCategories() {
            axios.get(`${baseURL}/api/products/categories`)
                .then(res => {
                    this.categories = res.data;
                })
                .catch(err => console.log(err));
        },
        scrollRight(){
            this.$refs.list.scrollLeft -= 250;
        },
        scrollLeft(){
            this.$refs.list.scrollLeft += 250;
        },
        // drag() {
        //     this.ul.addEventListener('mouseover', () => {
        //         this.translateX = parseInt(ul.style.transform.split('px, ')[0].substr(13) * (-1));
        //     });
        //     Draggable.create("#drag", {
        //         type: "x",
        //         bounds: {minX: 150, maxX: this.ul.offsetWidth * (-1)},
        //     });
        // }
    }
}
</script>

<style lang="scss" scoped>
section.categories {
    position: relative;
    

    .selector {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        height: 100%;
        width: 200px;
        z-index: 1;
        display: flex;
        justify-content: center;
        align-items: center;

        .btn {
            position: relative;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        &.left {
            left: 0;
            background: linear-gradient(to right, rgba(255,255,255,.9) 0%, rgba(255,255,255,.5) 70%, rgba(255,255,255,0) 100%);

            .btn {
                right: 20px;
            }
        }

        &.right {
            right: 0;
            background: linear-gradient(to left, rgba(255,255,255,.9) 0%, rgba(255,255,255,.5) 70%, rgba(255,255,255,0) 100%);

            .btn {
                left: 20px;
            }
        }
    }

    ul {
        list-style: none;
        display: flex;
        flex-wrap: nowrap;
        align-items: center;
        margin: 0 90px;
        padding: 0 50px;
        overflow: hidden;
        scroll-behavior: smooth;
       /*  &::-webkit-scrollbar {
                display: none;
            } */
    }
}
</style>