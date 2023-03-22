<script>
export default {
    methods: {
        async uploadFile(file) {
            const formData = new FormData();
            formData.append("file", file);
            const url = `${process.env.VUE_APP_API_URL}api/v1/media/upload`;

            try {

                const response = await this.axios.post(url, formData, {
                    headers: { "Content-Type": "multipart/form-data" },
                })
                return response.data.data.Location
            } catch (error) {
                console.log(error)
            }
        },
    },
};
</script>
