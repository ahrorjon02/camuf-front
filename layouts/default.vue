<template>
  <header class="header" :class="{ 'fixed-header': scrolledNav }">
    <div class="container">
      <NuxtLink to="/" class="site-logo">
        <img src="@/assets/images/png/logo.png" alt="" />
      </NuxtLink>
      <nav class="header-nav">
        <ul class="header-nav-list">
          <li>
            <NuxtLink to="/about">{{ $t("AboutUs") }}</NuxtLink>
          </li>
        </ul>
      </nav>
      <div class="lang">
        <div class="lang-top" @click="langOpen = !langOpen">{{ locale }}</div>
        <Transition name="lang">
          <ul class="lang-list" v-if="langOpen">
            <li
              v-for="{ name, code } in locales"
              :key="code"
              :style="`display: ${code == locale ? 'none' : 'flex'}`"
            >
              <NuxtLink
                @click="langOpen = false"
                :to="switchLocalePath(code)"
                >{{ name }}</NuxtLink
              >
            </li>
          </ul>
        </Transition>
      </div>
      <div
        @click="isOpenMenu = !isOpenMenu"
        class="header-menu-btn"
        :class="{ active: isOpenMenu }"
      >
        <span></span>
        <span></span>
        <span></span>
      </div>
    </div>
  </header>

  <div class="header-menu" :class="{ active: isOpenMenu }" ref="header_menu">
    <div class="header-menu__main flex items-start">
      <button class="header-menu__close" @click="isOpenMenu = false">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="40"
          height="40"
          viewBox="0 0 24 24"
        >
          <path
            :fill="dark ? '#fff' : '000'"
            d="m12 13.4l-4.9 4.9q-.275.275-.7.275t-.7-.275q-.275-.275-.275-.7t.275-.7l4.9-4.9l-4.9-4.9q-.275-.275-.275-.7t.275-.7q.275-.275.7-.275t.7.275l4.9 4.9l4.9-4.9q.275-.275.7-.275t.7.275q.275.275.275.7t-.275.7L13.4 12l4.9 4.9q.275.275.275.7t-.275.7q-.275.275-.7.275t-.7-.275z"
          />
        </svg>
      </button>
      <div class="header-menu__list h-full first-col">
        <h3 class="header-menu__title">{{ $t("Menues") }}</h3>
        <div
          class="header-menu__item"
          v-for="(menu, index) in menus"
          :key="`${index}`"
        >
          <NuxtLink
            class="header-menu__btn cursor-pointer"
            :class="{ active: index === activeMenu.one.id }"
            :to="menu.link ? menu.link : '/'"
            v-if="menu.isLink"
            @click="isOpenMenu = false"
            @mouseenter="
              (activeMenu.one = ''),
                (activeMenu.two = ''),
                (activeMenu.three = ''),
                (activeMenu.four = '')
            "
            v-html="menu.name"
          >
          </NuxtLink>
          <button
            v-else
            class="header-menu__btn"
            :class="{ active: menu.name === activeMenu.one.name }"
            @mouseenter="
              (activeMenu.one = menu),
                (activeMenu.two = ''),
                (activeMenu.three = ''),
                (activeMenu.four = '')
            "
          >
            {{ menu.name }}
            <img src="~/assets/images/svg/arrow-right.svg" />
          </button>
        </div>
      </div>
      <div class="header-menu__list menu-main h-full">
        <h3 class="header-menu__title">{{ activeMenu.one.name }}</h3>
        <div class="header-menu__items">
          <div
            class="header-menu__item"
            v-for="item in activeMenu.one.sub"
            :key="item"
          >
            <NuxtLink
              class="header-menu__btn"
              :to="{ path: item.link, query: { role: 'rahbariyat' } }"
              v-if="item.isLink && item.withQuery"
              @click="isOpenMenu = false"
              @mouseenter="
                (activeMenu.two = ''),
                  (activeMenu.three = ''),
                  (activeMenu.four = '')
              "
              v-html="item.name"
            ></NuxtLink>
            <NuxtLink
              class="header-menu__btn"
              :to="item.link ? item.link : '/'"
              v-else-if="item.isLink && !item.withQuery"
              @click="isOpenMenu = false"
              @mouseenter="
                (activeMenu.two = ''),
                  (activeMenu.three = ''),
                  (activeMenu.four = '')
              "
              v-html="item.name"
            ></NuxtLink>
            <!-- item -->
            <button
              v-else
              class="header-menu__btn"
              :class="{ active: item.name === activeMenu.two.name }"
              @mouseenter="
                (activeMenu.two = item),
                  (activeMenu.three = ''),
                  (activeMenu.four = '')
              "
            >
              {{ item.name }}
              <img src="~/assets/images/svg/arrow-right.svg" />
            </button>
          </div>
        </div>
      </div>
      <div class="header-menu__list menu-main h-full">
        <h3 class="header-menu__title">{{ activeMenu.two.name }}</h3>
        <div class="header-menu__items">
          <div
            class="header-menu__item"
            v-for="item in activeMenu.two.sub"
            :key="item"
          >
            <NuxtLink
              class="header-menu__btn"
              :to="item.link"
              v-if="item.isLink"
              @click="isOpenMenu = false"
              @mouseenter="(activeMenu.three = ''), (activeMenu.four = '')"
              v-html="item.name"
            ></NuxtLink>
            <!-- item -->
            <button
              v-else
              class="header-menu__btn"
              :class="{ active: item.name === activeMenu.three.name }"
              @mouseenter="(activeMenu.three = item), (activeMenu.four = '')"
            >
              {{ item.name }}
              <img src="~/assets/images/svg/arrow-right.svg" />
            </button>
          </div>
        </div>
      </div>
      <div class="header-menu__list menu-main h-full">
        <h3 class="header-menu__title">{{ activeMenu.three.name }}</h3>
        <div class="header-menu__items">
          <div
            class="header-menu__item"
            v-for="item in activeMenu.three.sub"
            :key="item"
          >
            <NuxtLink
              class="header-menu__btn cursor-pointer"
              :to="item.link"
              v-if="item.isLink"
              @click="isOpenMenu = false"
              @mouseenter="activeMenu.four = ''"
              v-html="item.name"
            ></NuxtLink>
            <!-- item -->
            <button
              v-else
              class="header-menu__btn"
              :class="{ active: item.name === activeMenu.four.name }"
              @mouseenter="activeMenu.four = item"
            >
              {{ item.name }}
              <img src="~/assets/images/svg/arrow-right.svg" />
            </button>
          </div>
        </div>
      </div>
      <div class="header-menu__list menu-main h-full">
        <h3 class="header-menu__title">{{ activeMenu.four.name }}</h3>
        <div class="header-menu__items">
          <div
            class="header-menu__item"
            v-for="item in activeMenu.four.sub"
            :key="item"
          >
            <NuxtLink
              class="header-menu__btn"
              @click="isOpenMenu = false"
              :to="item.link"
              v-if="item.isLink"
              v-html="item.name"
            ></NuxtLink>
            <!-- item -->
          </div>
        </div>
      </div>
    </div>

    <div class="small-menu__main">
      <div class="header-menu__list h-full first-col">
        <div class="header-menu__head flex items-center justify-between">
          <button
            class="flex items-center justify-start gap-2 header-menu__back"
            v-if="current.name"
            @click="backPreviusMenu(current.col)"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 20 20"
            >
              <path
                :fill="dark ? '#fff' : '000'"
                d="m5.83 9l5.58-5.58L10 2l-8 8l8 8l1.41-1.41L5.83 11H18V9z"
              />
            </svg>
            <!-- :fill="dark ? '#fff' : '000'" -->
            <span>Orqaga</span>
          </button>
          <h3 class="header-menu__title mr-8" v-if="current.name">
            {{ current.name }}
          </h3>
          <button
            class="header-menu__close"
            :class="{ 'ml-auto': !current.name }"
            @click="isOpenMenu = false"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="32"
              height="32"
              viewBox="0 0 24 24"
            >
              <path
                :fill="dark ? '#fff' : '000'"
                d="m12 13.4l-4.9 4.9q-.275.275-.7.275t-.7-.275q-.275-.275-.275-.7t.275-.7l4.9-4.9l-4.9-4.9q-.275-.275-.275-.7t.275-.7q.275-.275.7-.275t.7.275l4.9 4.9l4.9-4.9q.275-.275.7-.275t.7.275q.275.275.275.7t-.275.7L13.4 12l4.9 4.9q.275.275.275.7t-.275.7q-.275.275-.7.275t-.7-.275z"
              />
            </svg>
          </button>
        </div>
        <div
          class="header-menu__item"
          v-for="(menu, index) in smallMenu"
          :key="index"
        >
          <NuxtLink
            class="header-menu__btn cursor-pointer"
            :class="{ active: index === activeMenu.one.id }"
            :to="{ path: menu.link, query: { role: 'rahbariyat' } }"
            v-if="menu.isLink && menu.withQuery"
            @click="(isOpenMenu = false), (smallMenu = menus)"
            v-html="menu?.name"
          >
          </NuxtLink>
          <NuxtLink
            class="header-menu__btn cursor-pointer"
            :class="{ active: index === activeMenu.one.id }"
            :to="menu.link"
            v-else-if="menu.isLink && !menu.withQuery"
            @click="(isOpenMenu = false), (smallMenu = menus)"
            v-html="menu.name"
          >
          </NuxtLink>
          <button
            v-else
            class="header-menu__btn"
            @click="changeMenu(menu, index)"
          >
            {{ menu.name }}
            <img src="~/assets/images/svg/arrow-right.svg" />
          </button>
        </div>
      </div>
    </div>
  </div>

  <label class="relative inline-flex items-center cursor-pointer dark-mode">
    <input
      type="checkbox"
      value=""
      class="sr-only peer"
      :checked="dark"
      @change="darkChange()"
    />
    <div
      class="w-11 h-6 bg-gray-200 rounded-full peer peer-focus:ring-4 peer-focus:ring-blue-300 dark:peer-focus:ring-blue-800 dark:bg-gray-700 peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-0.5 after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all dark:border-gray-600 peer-checked:bg-blue-600"
    ></div>
  </label>

  <NuxtPage />
  <footer class="footer">
    <div class="container">
      <NuxtLink to="/" class="site-logo">
        <img src="@/assets/images/png/logo.png" alt="" />
      </NuxtLink>
      <ul class="footer__list">
        <li>
          <a href="tel:+998 95 485 00 70">+998 95 485 00 70</a>
        </li>
        <li>
          <a href="#">{{ $t("location") }}</a>
        </li>
        <li>
          <a href="mailto:info@camuf.uz"> info@camuf.uz </a>
        </li>
      </ul>
      <div class="footer__contact">
        <h2 class="footer__contact-title">{{ $t("LastNews") }}</h2>
        <p class="footer__contact-desc">
          {{ $t("FooterText") }}
        </p>
        <form @submit.prevent="" class="footer__contact-form">
          <input :placeholder="$t('Email')" type="email" />
          <button class="text-nowrap">{{ $t("FooterBtn") }}</button>
        </form>
      </div>
    </div>
  </footer>
</template>

<script setup>
//============================================ imports ============================================
// menus
// import menus from '~/utils/menus.js';
import Service from "~/services/Service";
import { useStore } from "~/store/store";
const { locale, locales, t } = useI18n();
const switchLocalePath = useSwitchLocalePath();
const store = useStore();
const scrolledNav = ref(false);
const updateScroll = () => {
  const scrollposition = window.scrollY;
  if (scrollposition > 800) {
    scrolledNav.value = true;
    return;
  }
  scrolledNav.value = false;
};
const dark = ref(localStorage.getItem("dark") ? true : false);
const langOpen = ref(false);
function darkChange() {
  if (localStorage.getItem("dark")) {
    localStorage.removeItem("dark");
    dark.value = false;
    document.querySelector("html").classList.remove("dark");
  } else {
    localStorage.setItem("dark", true);
    dark.value = true;
    document.querySelector("html").classList.add("dark");
  }
}
if (localStorage.getItem("dark")) {
  document.querySelector("html").classList.add("dark");
}
onMounted(async () => {
  window.addEventListener("scroll", updateScroll);

  await getTranslations();
  window.addEventListener("click", (e) => {
    if (!e.target.classList.contains("lang-top")) {
      langOpen.value = false;
    }
  });
});

watch(locale, async () => {
  await getTranslations();
});

//============================================ menus array ============================================

const menus = ref();

// function
function translateMenu() {
  menus.value = [
    {
      isLink: true,
      // name: 'Asosiy Sahifa',
      name: t("MainPage"),

      link: "/",
    },
    {
      isLink: false,
      // name: 'Universitet haqida',
      name: t("AboutTheUniversity"),
      link: "/about/",
      sub: [
        {
          isLink: true,
          name: t("Leadership"),
          // link: "{ path: '/staff/', query: { role: 'professor' }}",
          link: "staff",
          withQuery: true,
        },
        {
          isLink: false,
          // name: 'Universitet haqida',
          name: t("AboutTheUniversity"),
          link: "/about/",
          sub: [
            {
              isLink: true,
              name: t("Leadership"),
              // link: "{ path: '/staff/', query: { role: 'professor' }}",
              link: "staff",
              withQuery: true,
            },
            {
              isLink: false,
              name: t("Sections"),
              sub: [],
            },
            {
              isLink: false,
              name: t("DepartmentOfSpirituality"),
              sub: [
                {
                  isLink: false,
                  name: t("ScientificDepartment"),
                  sub: [
                    {
                      isLink: true,
                      name: t("ActaCAMUScientificJournal"),
                      link: "/journal/",
                    },
                    {
                      isLink: true,
                      name: t("ActaCAMuArchive"),
                      link: "/journal/",
                    },
                  ],
                },
              ],
            },
            {
              isLink: false,
              name: t("Faculties"),
              sub: [
                {
                  isLink: false,
                  name: t("FacultyOfMedicine"),
                  sub: [
                    {
                      isLink: false,
                      name: t("Departments"),
                      sub: [],
                    },
                  ],
                },
              ],
            },
          ],
        },
        {
          isLink: false,
          name: t("DepartmentOfSpirituality"),
          sub: [
            {
              isLink: false,
              name: t("ScientificDepartment"),
              sub: [
                {
                  isLink: true,
                  name: t("ActaCAMUScientificJournal"),
                },
                {
                  isLink: true,
                  name: t("ActaCAMuArchive"),
                },
                {
                  isLink: true,
                  name: t("JournalRequirements"),
                },
              ],
            },
          ],
        },
        {
          isLink: false,
          name: t("Faculties"),
          sub: [
            {
              isLink: false,
              name: t("FacultyOfMedicine"),
              sub: [
                {
                  isLink: false,
                  name: t("Departments"),
                  sub: [],
                },
              ],
            },
          ],
        },
      ],
    },
    {
      isLink: false,
      name: t("Directions"),
      sub: [],
    },
    {
      isLink: false,
      name: t("News"),
      sub: [],
    },
    {
      isLink: false,
      name: t("Journals"),
      sub: [],
    },
    {
      isLink: false,
      name: t("Communication"),
      sub: [
        {
          isLink: "true",
          name: t("LeaveMessage"),
          link: "/connect/",
        },
        {
          isLink: "true",
          name: t("ContactInformation"),
          link: "/contact/",
        },
      ],
    },
    {
      isLink: false,
      name: t("ElectronicResources"),
      sub: [
        {
          isLink: "true",
          name: t("ElectronicLibrary"),
        },
        {
          isLink: false,
          name: t("Communication"),
          sub: [
            {
              isLink: "true",
              name: t("LeaveMessage"),
              link: "/connect/",
            },
            {
              isLink: "true",
              name: t("ContactInformation"),
              link: "/contact/",
            },
          ],
        },
        {
          isLink: false,
          name: t("ElectronicResources"),
          sub: [
            {
              isLink: "true",
              name: t("Portfolio"),
              link: "http://192.168.1.145",
            },
            {
              isLink: "true",
              name: t("ElectronicLibrary"),
              link: "/journal/",
            },
            {
              isLink: "true",
              name: t("OnlineLearningPlatform"),
              link: "/journal/",
            },
            {
              isLink: "true",
              name: t("HEMIS"),
              link: "https://hemis.camuf.uz/dashboard/login",
            },
            {
              isLink: "true",
              name: t("ConfirmationDiploma"),
            },
            {
              isLink: false,
              name: t("ElectronicLinks"),
              sub: [
                {
                  isLink: "true",
                  name: "president.uz",
                  link: "https://president.uz/",
                },
                {
                  isLink: "true",
                  name: "gov.uz",
                  link: "https://www.gov.uz/uz",
                },
                {
                  isLink: "true",
                  name: "ferghana.uz",
                  link: "http://ferghana.uz/",
                },
                {
                  isLink: "true",
                  name: "my.gov.uz",
                  link: "https://my.gov.uz/",
                },
                {
                  isLink: "true",
                  name: "ssv.ssv.uz",
                  link: "https://ssv.ssv.uz/",
                },
                {
                  isLink: "true",
                  name: "edu.uz",
                  link: "http://edu.uz/uz",
                },
                {
                  isLink: "true",
                  name: "ziyonet.uz",
                  link: "http://ziyonet.uz/",
                },
                {
                  isLink: "true",
                  name: "lex.uz",
                  link: "https://lex.uz/",
                },
                {
                  isLink: "true",
                  name: "Medical Planet",
                  link: "https://www.youtube.com/@medic_planet",
                },
              ],
            },
          ],
        },
        {
          isLink: false,
          name: t("Admissions"),
          sub: [
            {
              isLink: "true",
              name: t("about_admission"),
              link: "/admission/",
            },
          ],
        },
        {
          isLink: "false",
          name: t("ElectronicLinks"),
          sub: [
            {
              isLink: "true",
              name: "https://president.uz/",
              link: "https://president.uz/",
            },
            {
              isLink: "true",
              name: "https://www.gov.uz/uz",
              link: "https://www.gov.uz/uz",
            },
            {
              isLink: "true",
              name: "http://ferghana.uz/",
            },
            {
              isLink: "true",
              name: "https://my.gov.uz/",
            },
            {
              isLink: "true",
              name: "http://www.minzdrav.uz/",
            },
            {
              isLink: "true",
              name: "http://edu.uz/uz",
            },
            {
              isLink: "true",
              name: "http://ziyonet.uz/",
            },
            {
              isLink: "true",
              name: "http://www.lex.uz/",
            },
          ],
        },
      ],
    },
    {
      isLink: true,
      link: "/admission/",
      name: t("Acceptance"),
    },
    {
      isLink: true,
      link: "/vacancy/",
      name: t("vacancy"),
    },
  ];
}

const smallMenu = ref();

async function getTranslations() {
  await translateMenu();
  await getSections();
  await getDepartaments();
  await getNewsCategories();
  await getCourseCategories();
  await getArticleCategories();
  // smal menus
  smallMenu.value = menus.value;
}
//============================================ menus array ============================================

const news_categories = ref({});
async function getNewsCategories() {
  const res = await Service.getNewsCategories(locale.value);
  news_categories.value = res.data.results;

  news_categories.value.forEach((category) => {
    category.isLink = true;
    category.link = `/news/${category.id}/`;
  });
  const menuIndex = menus.value.findIndex((item) => item.name === t("News"));
  menus.value[menuIndex].sub = news_categories.value;
}

const course_categories = ref({});
async function getCourseCategories() {
  const res = await Service.getCourseCategories(locale.value);
  course_categories.value = res.data.results;

  course_categories.value.forEach((category) => {
    category.isLink = true;
    category.link = `/course/${category.id}/`;
  });
  const menuIndex = menus.value.findIndex(
    (item) => item.name === t("Directions")
  );
  menus.value[menuIndex].sub = course_categories.value;
}

const article_categories = ref({});
async function getArticleCategories() {
  const res = await Service.getArticleCategories(locale.value);
  store.articles = res.data;
  store.articlesItems = res.data;
  store.articles?.results.forEach((category) => {
    category.isLink = true;
    category.link = `/journals/${category.id}/`;
  });
  const menuIndex = menus.value.findIndex(
    (item) => item.name === t("Journals")
  );
  menus.value[menuIndex].sub = store.articles?.results;
}

const departaments = ref({});
async function getDepartaments() {
  const res = await Service.getDepartaments(locale.value);
  store.articles = res.data;

  console.log(res.data);
  store.articles?.results.forEach((category) => {
    category.isLink = true;
    category.link = `/departments/${category.id}/`;
  });
  const menuIndex = menus.value.findIndex(
    (item) => item.name === t("AboutTheUniversity")
  );
  menus.value[menuIndex].sub[3].sub[0].sub[0].sub = store.articles?.results;
}

const sections = ref({});
async function getSections() {
  const res = await Service.getSections(locale.value);
  store.sections = res.data;

  store.sections?.results.forEach((category) => {
    category.isLink = true;
    category.link = `/sections/${category.id}/`;
  });
  const menuIndex = menus.value.findIndex(
    (item) => item.name === t("AboutTheUniversity")
  );
  menus.value[menuIndex].sub[1].sub = store.sections?.results;
}

//============================================ header menu ============================================
//variables
const activeMenu = reactive({
  one: {},
  two: "",
  three: "",
  four: "",
  five: "",
});

const previus = ref([]);
const current = ref({});
const all = ref([]);
const index = ref([]);
// is menu open
const isOpenMenu = ref(false);

// watch(isOpenMenu, () => {
//     if (isOpenMenu.value) {
//         document.body.style.maxHeight = '100vh'
//         document.body.style.overflow = 'hidden'
//     } else {
//         document.body.style.maxHeight = 'auto'
//         document.body.style.overflow = 'auto'
//     }
// })

// functions
function changeMenu(menu, i) {
  smallMenu.value = menu.sub;
  current.value = menu;
  index.value.push(i);
}

function backPreviusMenu(col) {
  switch (index.value.length) {
    case 1: {
      smallMenu.value = menus.value;
      current.value = { name: "" };
      index.value.pop();
      break;
    }
    case 2: {
      smallMenu.value = menus.value[index.value[0]].sub;
      current.value = menus.value[index.value[0]];
      index.value.pop();
      break;
    }
    case 3: {
      smallMenu.value = menus.value[index.value[0]].sub[index.value[1]].sub;
      current.value = menus.value[index.value[0]].sub[index.value[1]];
      index.value.pop();
      break;
    }
    case 4: {
      smallMenu.value =
        menus.value[index.value[0]].sub[index.value[1]].sub[index.value[2]].sub;
      current.value =
        menus.value[index.value[0]].sub[index.value[1]].sub[index.value[2]];
      index.value.pop();
      break;
    }
  }
}

//functions
</script>

<style lang="scss">
.lang-enter-active,
.lang-leave-active {
  transition: all 0.5s ease;
  transform: translateY(0);
}

.lang-enter-from,
.lang-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}
</style>
