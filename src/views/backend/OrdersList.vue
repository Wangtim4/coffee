<template>
    <table class="table mt-4">
        <thead>
            <tr>
                <th>購買時間</th>
                <th>Email</th>
                <th>購買款項</th>
                <th>應付金額</th>
                <th>是否付款</th>
                <th>編輯</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(item, key) in orders" :key="key">
                <td>{{ item.create_at }}</td>
                <td>{{ item.user.email }}</td>
                <td>
                    <ul class="list-unstyled">
                        <li v-for="(product, i) in item.products" :key="i">
                            {{ product.product.title }} 數量：{{ product.qty }}
                            {{ product.product.unit }}
                        </li>
                    </ul>
                </td>
                <td>{{ item.total }}</td>
                <td>
                    <div class="form-check form-switch">
                        <input class="form-check-input" type="checkbox" :id="`paidSwitch${item.id}`" v-model="item.is_paid"
                            @change="updatePaid(item)">
                        <label class="form-check-label" :for="`paidSwitch${item.id}`">
                            <span v-if="item.is_paid">已付款</span>
                            <span v-else>未付款</span>
                        </label>
                    </div>
                </td>
                <td>
                    <div class="btn-group">
                        <button class="btn btn-outline-primary btn-sm" @click="openModal()">檢視</button>
                        <button class="btn btn-outline-danger btn-sm" @click="openDelOrderModal(i)">刪除</button>
                    </div>
                </td>
            </tr>
        </tbody>
    </table>
</template>
<script>

export default {
    data() {
        return {
            orders: {},
            isNew: false,
            pagination: {},
            tempOrder: {},
            currentPage: 1,
        };
    },
    methods: {
        getOrder(currentPage = 1) {
            this.currentPage = currentPage;
            // [API]: /api/:api_path/admin/orders?page=:page[方法]: get
            const url = `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/admin/orders`;
            this.$http.get(url, this.tempOrder).then((res) => {
                console.log(res);
                this.orders = res.data.orders;
                this.pagination = res.data.pagination;
            })
        },
    },
    created() {
        this.getOrder();
    },
}
</script>