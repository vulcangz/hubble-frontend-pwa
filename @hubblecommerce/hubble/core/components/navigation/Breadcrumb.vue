<template>
    <div class="breadcrumb-wrp">
        <div class="breadcrumb">
            <ul class="row">
                <li class="breadcrumb-item">
                    <nuxt-link to="/" title="Back to Home" v-text="'Home'" />
                </li>

                <li v-for="(element, index) in path" :key="index" class="breadcrumb-item">
                    <nuxt-link :to="element.url">
                        <span itemprop="title">{{ element.name }}</span>
                    </nuxt-link>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Breadcrumb',

    props: {
        path: {
            required: true,
            type: Array,
        },
    },

    head() {
        if (this.path.length > 0) {
            let currentPath = [];

            this.path.forEach((pathItem, key) => {
                currentPath.push({
                    '@type': 'ListItem',
                    'position': key + 1,
                    'name': pathItem.name,
                    'item': process.env.APP_BASE_URL + pathItem.url,
                });
            });

            let structuredDataBreadcrumbs = {
                '@context': 'https://schema.org',
                '@type': 'BreadcrumbList',
                'itemListElement': currentPath,
            };

            return {
                script: [{ json: structuredDataBreadcrumbs, type: 'application/ld+json' }],
            };
        }
    },
};
</script>

<style lang="scss">
.breadcrumb {
    font-size: 14px;
    line-height: 17px;
    text-align: center;
    padding: 15px 0;

    ul {
        align-items: center;
        list-style: none;
        margin: 0;
        padding: 0;

        li {
            margin-right: 5px;
        }
    }

    .row {
        .breadcrumb-item:first-child {
            .nuxt-link-active {
                font-weight: normal;
            }
        }
    }

    .breadcrumb-item {
        display: block;
        display: -webkit-box;
        hyphens: auto;
        max-width: 250px;
        text-overflow: ellipsis;
        -webkit-line-clamp: 1;
        -webkit-box-orient: vertical;
        overflow: hidden;
        text-align: left;

        a {
            vertical-align: middle;
        }

        .nuxt-link-active {
            font-weight: normal;
        }
    }

    .breadcrumb-item + .breadcrumb-item {
        padding-left: 0;
    }

    .breadcrumb-item + .breadcrumb-item::before {
        content: ' / ';
        width: 6px;
        display: inline-block;
        margin-right: 5px;
        padding: 0;
        vertical-align: middle;
    }
}

@media (min-width: 768px) {
    .breadcrumb-wrp {
        width: 100%;
    }

    .breadcrumb {
        display: block;
    }
}
</style>
