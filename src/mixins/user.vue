<script>
import config from '../config';
import { Http } from "@capacitor-community/http";
export default ({

    data() {
        return {
            userData: { name: '' }
        }
    },
    mounted() {
        const userInfo = JSON.parse(localStorage.getItem('key'));

        if (!userInfo || !userInfo._id) {
            this.getUserData()
        }
        else {
            this.userData = userInfo;

        }
    },
    methods: {

        getUserData() {

            const url = config.api + 'api/v1/user';
            const token = localStorage.getItem('token');

            Http.get({
                url,
                headers: {
                    'Content-Type': 'application/json',
                    Authorization: token
                }
            })
                .then(response => {
                    if (response.status) {
                        this.userData = response.data.user;
                        console.log(response.data);
                    }
                })
                .catch(error => {
                    console.log(error);
                });
        }
    }

})
</script>