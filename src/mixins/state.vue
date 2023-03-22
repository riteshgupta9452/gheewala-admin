<script>
export default ({

    data() {
        return {
            states: [{
                state_name: 'Andhra Pradesh',
                cities: [
                    'Amaravati',
                    'Visakhapatnam',
                    'Vijayawada',
                    'Guntur',
                    'Nellore',
                ],



            },
            {
                state_name: 'Arunachal Pradesh',
                cities: ['Itanagar']
            },
            {
                state_name: 'Assam',
                cities: ['Guwahati',
                    'Dibrugarh',
                    'Jorhat',
                    'Silchar',
                ]
            },
            {
                state_name: 'Bihar',

                cities: [


                    'Patna',
                    'Gaya',
                    'Bhagalpur',
                    'Muzaffarpur',
                ]
            },
            {
                state_name: 'Chhattisgarh',
                cities: ['Raipur',
                    'Bilaspur',
                    'Korba',
                    'Bhilai']
            },

            {
                state_name: 'Goa',
                cities:

                    ['Panaji',
                        'Madgaon',
                        'Mapusa']
            },

            {
                state_name: 'Gujarat',
                cities: [
                    'Ahmedabad',
                    'Surat',
                    'Vadodara',
                    'Rajkot',
                    'Gandhinagar']
            },

            {
                state_name: 'Haryana',
                cities: [
                    'Ambala',
                    'Bhiwani',
                    'Charkhi Dadri',
                    'Faridabad',
                    'Fatehabad',
                    'Gurugram',
                    'Hisar',
                    'Jhajjar',
                    'Jind',
                    'Kaithal',
                    'Karnal',
                    'Kurukshetra',
                    'Mahendragarh',
                    'Nuh',
                    'Palwal',
                    'Panchkula',
                    'Panipat',
                    'Rewari',
                    'Rohtak',
                    'Sirsa',
                    'Sonipat',
                    'Yamunanagar'
                ]
            },
            {
                state_name: 'Himachal Pradesh',
                cities: [
                    'Shimla',
                    'Dharamshala',
                    'Kullu',
                    'Manali',
                    'Palampur',
                    'Mandi',
                    'Solan',
                    'Bilaspur',
                    'Hamirpur',
                    'Una',
                    'Kangra',
                    'Chamba',
                    'Lahaul and Spiti',
                    'Kinnaur'
                ]
            },
            {
                state_name: 'Jharkhand',
                cities: [
                    'Ranchi',
                    'Jamshedpur',
                    'Dhanbad',
                    'Bokaro Steel City',
                    'Deoghar',
                    'Phusro',
                    'Hazaribagh',
                    'Giridih',
                    'Ramgarh',
                    'Medininagar(Daltonganj)',
                    'Chaibasa',
                    'Chatra',
                    'Garhwa'
                ]
            },
            {
                state_name: 'Karnataka',
                cities: [
                    'Bengaluru',
                    'Mysore',
                    'Hubli-Dharwad',
                    'Gulbarga',
                    'Belgaum',
                    'Mangalore',
                    'Davanagere',
                    'Bellary',
                    'Bijapur',
                    'Shimoga',
                    'Tumkur',
                    'Raichur',
                    'Hassan',
                    'Chikmagalur',
                    'Mandya',
                    'Udupi'
                ]
            },
            {
                state_name: 'Kerala',
                cities: [
                    'Thiruvananthapuram',
                    'Kochi',
                    'Kozhikode',
                    'Thrissur',
                    'Kannur',
                    'Palakkad',
                    'Alappuzha',
                    'Ernakulam',
                    'Kollam',
                    'Kottayam',
                    'Pathanamthitta',
                    'Idukki',
                    'Malappuram',
                    'Wayanad',
                    'Kasaragod'
                ]
            },
            {
                state_name: 'Madhya Pradesh',
                cities: [
                    'Bhopal',
                    'Indore',
                    'Jabalpur',
                    'Gwalior',
                    'Ujjain',
                    'Sagar',
                    'Dewas',
                    'Satna',
                    'Ratlam',
                    'Rewa',
                    'Murwara (Katni)',
                    'Pithampur',
                    'Singrauli',
                    'Khandwa',
                    'Mandsaur',
                    'Chhindwara',
                    'Vidisha'
                ]
            },
            {
                state_name: 'Maharashtra',
                cities: [
                    'Mumbai',
                    'Pune',
                    'Nagpur',
                    'Nashik',
                    'Thane',
                    'Solapur',
                    'Aurangabad',
                    'Kalyan-Dombivali',
                    'Vasai-Virar',
                    'Pimpri-Chinchwad',
                    'Akola',
                    'Amravati',
                    'Bhiwandi',
                    'Ichalkaranji',
                    'Latur',
                    'Sangli-Miraj & Kupwad',
                    'Dhule',
                    'Jalgaon',
                    'Kolhapur',
                    'Panvel'
                ]
            },
            {
                state_name: 'Manipur',
                cities: ['Imphal'],
            },
            {
                state_name: 'Meghalaya',
                cities: ['Shillong'],
            },
            {
                state_name: 'Mizoram',
                cities: ['Aizawl'],
            },
            {
                state_name: 'Nagaland',
                cities: ['Kohima'],
            },
            {
                state_name: 'Odisha',
                cities: ['Bhubaneswar'],
            },
            {
                state_name: 'Punjab',
                cities: [
                    'Ludhiana',
                    'Amritsar',
                    'Jalandhar',
                    'Patiala',
                    'Bathinda',
                    'SAS Nagar',
                    'Hoshiarpur',
                    'Firozpur',
                    'Pathankot',
                    'Moga',
                    'Batala',
                    'Muktsar',
                    'Abohar',
                    'Barnala',
                    'Rajpura',
                    'Khanna',
                    'Phagwara',
                    'Sunam'
                ]
            },
            {
                state_name: 'Rajasthan',
                cities: ['Jaipur'],
            },
            {
                state_name: 'Sikkim',
                cities: ['Gangtok'],
            },
            {
                state_name: 'Tamil Nadu',
                cities: ['Chennai'],
            },
            {
                state_name: 'Telangana',
                cities: ['Hyderabad'],
            },
            {
                state_name: 'Tripura',
                cities: ['Agartala'],
            },
            {
                state_name: 'Uttar Pradesh',
                cities: [
                    'Lucknow',
                    'Kanpur',
                    'Agra',
                    'Varanasi',
                    'Meerut',
                    'Allahabad',
                    'Gorakhpur',
                    'Ghaziabad',
                    'Rae Bareli',
                    'Moradabad',
                    'Aligarh',
                    'Saharanpur',
                    'Jhansi',
                    'Bareilly',
                    'Firozabad',
                    'Rampur',
                    'Mainpuri',
                    'Etawah',
                    'Budaun',
                    'Mathura'
                ]
            },

            {
                state_name: 'Uttarakhand',
                cities: ['Dehradun'],
            }


            ],

        }
    },
})
</script>
