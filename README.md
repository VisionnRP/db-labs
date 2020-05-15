Лабораторна робота No 1. Вивчення базових операцій обробки XML-документів
Завдання
Постановка завдання
Завдання 1: www.uahotels.info
Завдання 2:Мінімальна кількість графічних фрагментів
Завдання 3: www.zvetsad.com.ua

Варіант 18

Оскільки я зіткнувся з проблемою 403, я не зміг виконати парсинг данних з сайту свого варіанту, тому я обрав варіант 17, та виконав лабораторну роботу. Зміст завдання 2 обрав з 18 варіанту.

Лістинг коду:



from scrapy.http.response import Response
import scrapy


uaholels.py
class UaHotels(scrapy.Spider):
    name = 'uahotels'
    allowed_domains = ['uahotels.info']
    start_urls = ['https://uahotels.info/']

    def parse(self, response: Response):
        all_images = response.xpath("//img/@src[starts-with(., 'https')]")
        yield {'url': response.url, 'payload': [{'type': 'image', 'data': image.get()} for image in all_images]}
        if response.url == self.start_urls[0]:
            all_links = response.xpath("//a/@href[starts-with(., 'https://uahotels.info/')]")
            selected_links = [link.get() for link in all_links][:19]
            for link in selected_links:
                yield scrapy.Request(link, self.parse)


class Zvetsad(scrapy.Spider):
    name = 'zvetsad'
    allowed_domains = ['www.zvetsad.com.ua']
    start_urls = ['https://www.zvetsad.com.ua/catalog/rozyi']

    def parse(self, response: Response):
        products = response.xpath("//div[contains(@class, 'item_div')]")[:20]
        for files in range(20):
            yield {
                'description': products.xpath("//div[contains(@class, 'item_nazvanie')]/a/text()").extract()[files],
                'price': products.xpath("//div[contains(@class, 'price fl')]/text()").extract()[files],
                'img': products.xpath("//img/@src").extract()[files],
            }
main.py
from scrapy.crawler import CrawlerProcess
from scrapy.utils.project import get_project_settings
from lxml import etree
import os
import webbrowser


if __name__ == '__main__':
    try:
        os.remove("task1.xml")
        os.remove("task2.xml")
        os.remove("task2.xhtml")
    except OSError:
        pass
        process = CrawlerProcess(get_project_settings())
        process.crawl('uahotels')
        process.crawl('zvetsad')
        process.start()
    while True:
        print("-" * 45)
        print("Choose 1 or 2")
        print("1")
        print("2")
        print("=: ", end='', flush=True)
        number = input()
        if number == "1":
            print("Task1")
            root = etree.parse("task1.xml")
            pages = root.xpath("//page")
            minImagePages = {}
            lastMinimal = 100
            for page in pages:
                url = page.xpath("@url")[0]
                count = page.xpath("count(fragment[@type='image'])")
                if count < lastMinimal:
                    lastMinimal = count
                    minImagePages = {url, count}

            print(minImagePages)
        elif number == "2":
            print("Task2")
            transform = etree.XSLT(etree.parse("task2.xsl"))
            result = transform(etree.parse("task2.xml"))
            result.write("task2.xhtml", pretty_print=True, encoding="UTF-8")
            webbrowser.open('file://' + os.path.realpath("task2.xhtml"))
        else:
            break
task1.xml
<?xml version='1.0' encoding='UTF-8'?>
<data>
  <page url="https://uahotels.info/">
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/5/a/15a62f5f63d2326b362588660fa48cb0.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/b/4/2b42e0b3d56e293dcbcca06ed7953e8f.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/d/5/5d5784196cc0ac8c256f2aa8c06f3d4b.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/c/5/cc5a903f975958d3c2212957f3b039eb.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/e/a/a/eaa5e7d498d603328cda0e7305711ca3.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/4/9/7/497d090e09df0a73ac4aa57e4e920ea0.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/9/a/6/9a696cdce702d07601eccb0b475b7063.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/b/8/b/b8b4a805b6bca889d1f6a54d4a753aea.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/4/8/d48c2798e36064c31e19cdc3ce044564.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/7/b/27b6681cfd8deaf1b3b326163cd021f9.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/f/9/5/f958800e1360f4457a2e2325ad4dbe1f.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/0/e/20e165687d9ab8ee621897a0dddbd373.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/f/9/5f945a94012e57a7ec5871c0f11848f5.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/f/3/a/f3a3187f5c191c38272ea940bb6aa3e9.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/0/7/5073ca6beccbb50f78a81be5cf2fdce9.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/4/d/6/4d66c86d4275f22f0d2d42ec7f014c7c.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/3/1/8/31872584ad54dd37951805a40c34ea39.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/a/a/caaddebb57e1e2e0b56c7db31ad1e8a8.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/9/d/a/9da9644e9315210badc4f106ab63c4d2.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/a/a/3/aa3494457a344b93ee982cac609128f1.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/8/8/a/88a4071286080cf6cbeef799091d9011.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/hotel/Sanatoriy-Novomoskovskiy-Orlovshchina/events/kottedzhnye-domiki---otkrytie-v-mae/">
    <fragment type="image">https://uahotels.info/user/data/images/load.gif</fragment>
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/cache/images/e/a/a/eaa5e7d498d603328cda0e7305711ca3.kottedzhnye-domiki---otkrytie-v-mae.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/d/5/5d5784196cc0ac8c256f2aa8c06f3d4b.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/c/5/cc5a903f975958d3c2212957f3b039eb.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/e/a/a/eaa5e7d498d603328cda0e7305711ca3.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/7/c/17c1172460ac174d7d2221f2f69e58b8.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/7/1/5/715fa4a87711dcd20ce21321a41763d5.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/e/b/2eb7e26d477b81df36a4c2161fb19e7f.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/7/e/a/7eace749f5dac85db2df001bd2810017.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/b/f/dbfe0bd2a05c6afce3b59399a12462ac.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/7/2/172902b2fcdd34d35a6b4c28d4978226.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/catalog/">
    <fragment type="image">https://uahotels.info/user/data/images/load.gif</fragment>
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/catalog/images/magnifier-left.png</fragment>
    <fragment type="image">https://uahotels.info/cache/images/0/c/5/0c53a9c2792e9255182753f58c28c59c.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/f/1/df1c5f1cae0c542ace70bf13324ac99d.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/e/b/2eb7e26d477b81df36a4c2161fb19e7f.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/a/b/9/ab9f6a8a2eb0f0baf068077913f11885.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/e/e/2/ee2c8a4819f4846596577f5a69d99e07.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/7/2/3/723fa2574bde9dc2c7cb96c70f4f3a93.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/a/0/d/a0d9cda21dca9c07b77c3c2242bfa355.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/0/4/f/04f8d29846e74d62f3d088d6947d32f4.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/8/8/f/88f3902afa62465625a6078a052aa160.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/2/c/22c0fcf1459c1c18be2a1020cd9ed894.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/5/a/15a62f5f63d2326b362588660fa48cb0.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/b/4/2b42e0b3d56e293dcbcca06ed7953e8f.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/d/5/5d5784196cc0ac8c256f2aa8c06f3d4b.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/f/1/df1c5f1cae0c542ace70bf13324ac99d.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/f/1/df1c5f1cae0c542ace70bf13324ac99d.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/e/b/2eb7e26d477b81df36a4c2161fb19e7f.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/hotel/Sanatoriy-Novomoskovskiy-Orlovshchina/events/pohudet-za-2-nedeli--programma-korrekcii-vesa-v-sanatorii/">
    <fragment type="image">https://uahotels.info/user/data/images/load.gif</fragment>
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/d/5/5d5784196cc0ac8c256f2aa8c06f3d4b.pohudet-za-2-nedeli--programma-korrekcii-vesa-v-sanatorii.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/d/5/5d5784196cc0ac8c256f2aa8c06f3d4b.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/c/5/cc5a903f975958d3c2212957f3b039eb.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/e/a/a/eaa5e7d498d603328cda0e7305711ca3.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/7/c/17c1172460ac174d7d2221f2f69e58b8.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/7/1/5/715fa4a87711dcd20ce21321a41763d5.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/e/b/2eb7e26d477b81df36a4c2161fb19e7f.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/7/e/a/7eace749f5dac85db2df001bd2810017.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/b/f/dbfe0bd2a05c6afce3b59399a12462ac.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/7/2/172902b2fcdd34d35a6b4c28d4978226.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/hotel/baza-otdyha-druzhba-staryy-saltov/events/mayskie-2020-na-bo-druzhba-stroyinvest-0507470273/">
    <fragment type="image">https://uahotels.info/user/data/images/load.gif</fragment>
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/b/4/2b42e0b3d56e293dcbcca06ed7953e8f.mayskie-2020-na-bo-druzhba-stroyinvest-0507470273.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/b/4/2b42e0b3d56e293dcbcca06ed7953e8f.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/a/6/9/a6955813c5559d03969c4d44334551f4.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/3/f/53f0d2f3f14d5575e87096322483ad15.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/a/c/3/ac3331a39f81be5d3784d97165b2ee62.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/7/e/9/7e9f6c619a9e7cc924dd547f76f0bee8.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/9/c/c/9ccb2c837d8bb335097bf346af30014f.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/3/a/3/3a36ae0d938b684bf4880224b74d2c28.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/3/c/6/3c6e38fa770e9f02aa3af3c158c8d1b4.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/8/6/1/861fa3a37ff0734177f1144e7ca197f6.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/3/4/d349b8b04498ffb020a182ce530286e0.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/info/">
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/cache/images/9/d/a/9da9644e9315210badc4f106ab63c4d2.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/a/a/3/aa3494457a344b93ee982cac609128f1.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/8/8/a/88a4071286080cf6cbeef799091d9011.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/a/3/1a36c1ff90c3b5902a680f6c0b175efe.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/8/5/1850368308358d22e60ab844f7d49e45.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/2/6/d26720f840ef426bb2d781c0840df06d.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/a/f/e/afe6036697777a02b8e353b57838e0ca.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/3/f/a/3fa233744c15b7ff7a22809a4d2e36b4.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/e/f/4/ef4685ff5bb9b0f8339d07455c3e4479.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/f/6/5/f65ad9d16c1d693043ef467df347cbf6.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/4/c/5/4c5853af42fd9c3b3c4e2631fac4e00d.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/c/b/ccb11e3ffacfbab2f02b3ecd1045df5b.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/f/2/7/f27c57be025c0994274325510936ac12.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/hotel/detskiye-lagerya-gorodskoy-lager-tvorcheskie-kanikuly-zaporozhe/events/nabor-v-gorodskoy-lager-tvorchestva/">
    <fragment type="image">https://uahotels.info/user/data/images/load.gif</fragment>
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/5/a/15a62f5f63d2326b362588660fa48cb0.nabor-v-gorodskoy-lager-tvorchestva.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/5/a/15a62f5f63d2326b362588660fa48cb0.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/6/8/2/68224a8b689b196b076f6d5816bc9b72.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/4/9/8/498fe5671363f134fa8bf55cb1d662ac.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/9/d/5/9d541331e3648b10d4385bb054ef2849.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/f/f/0/ff0d2213c0f6a483842ac209a817ef26.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/0/d/c/0dcf15b5b04e211390f5e1eca46d29e1.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/0/e/6/0e686b251a5004d70c395950abf9465e.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/9/b/c9b6dc4fa8857a2b977d54a0a6c9fe3b.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/news/">
    <fragment type="image">https://uahotels.info/user/data/images/load.gif</fragment>
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/5/a/15a62f5f63d2326b362588660fa48cb0.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/b/4/2b42e0b3d56e293dcbcca06ed7953e8f.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/d/5/5d5784196cc0ac8c256f2aa8c06f3d4b.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/c/5/cc5a903f975958d3c2212957f3b039eb.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/e/a/a/eaa5e7d498d603328cda0e7305711ca3.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/4/9/7/497d090e09df0a73ac4aa57e4e920ea0.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/9/a/6/9a696cdce702d07601eccb0b475b7063.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/b/8/b/b8b4a805b6bca889d1f6a54d4a753aea.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/4/8/d48c2798e36064c31e19cdc3ce044564.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/b/7/1/b71b75dcc960b5b1273fa1f70f444e60.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/7/b/27b6681cfd8deaf1b3b326163cd021f9.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/f/9/5/f958800e1360f4457a2e2325ad4dbe1f.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/0/e/20e165687d9ab8ee621897a0dddbd373.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/f/9/5f945a94012e57a7ec5871c0f11848f5.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/f/3/a/f3a3187f5c191c38272ea940bb6aa3e9.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/0/7/5073ca6beccbb50f78a81be5cf2fdce9.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/4/d/6/4d66c86d4275f22f0d2d42ec7f014c7c.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/map/">
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/hotel/Sanatoriy-Novomoskovskiy-Orlovshchina/events/lechenie-depressii-v-sanatorii/">
    <fragment type="image">https://uahotels.info/user/data/images/load.gif</fragment>
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/c/5/cc5a903f975958d3c2212957f3b039eb.lechenie-depressii-v-sanatorii.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/d/5/5d5784196cc0ac8c256f2aa8c06f3d4b.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/c/5/cc5a903f975958d3c2212957f3b039eb.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/e/a/a/eaa5e7d498d603328cda0e7305711ca3.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/7/c/17c1172460ac174d7d2221f2f69e58b8.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/7/1/5/715fa4a87711dcd20ce21321a41763d5.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/e/b/2eb7e26d477b81df36a4c2161fb19e7f.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/7/e/a/7eace749f5dac85db2df001bd2810017.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/b/f/dbfe0bd2a05c6afce3b59399a12462ac.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/7/2/172902b2fcdd34d35a6b4c28d4978226.120.120.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/user/account/registration/">
    <fragment type="image">https://uahotels.info/user/data/images/load.gif</fragment>
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/user/account/login/">
    <fragment type="image">https://uahotels.info/user/data/images/load.gif</fragment>
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
  <page url="https://uahotels.info/">
    <fragment type="image">https://uahotels.info/images/logo.32.32.png</fragment>
    <fragment type="image">https://uahotels.info/cache/images/1/5/a/15a62f5f63d2326b362588660fa48cb0.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/b/4/2b42e0b3d56e293dcbcca06ed7953e8f.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/d/5/5d5784196cc0ac8c256f2aa8c06f3d4b.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/c/5/cc5a903f975958d3c2212957f3b039eb.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/e/a/a/eaa5e7d498d603328cda0e7305711ca3.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/4/9/7/497d090e09df0a73ac4aa57e4e920ea0.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/9/a/6/9a696cdce702d07601eccb0b475b7063.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/b/8/b/b8b4a805b6bca889d1f6a54d4a753aea.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/d/4/8/d48c2798e36064c31e19cdc3ce044564.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/7/b/27b6681cfd8deaf1b3b326163cd021f9.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/f/9/5/f958800e1360f4457a2e2325ad4dbe1f.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/2/0/e/20e165687d9ab8ee621897a0dddbd373.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/f/9/5f945a94012e57a7ec5871c0f11848f5.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/f/3/a/f3a3187f5c191c38272ea940bb6aa3e9.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/5/0/7/5073ca6beccbb50f78a81be5cf2fdce9.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/4/d/6/4d66c86d4275f22f0d2d42ec7f014c7c.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/3/1/8/31872584ad54dd37951805a40c34ea39.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/c/a/a/caaddebb57e1e2e0b56c7db31ad1e8a8.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/9/d/a/9da9644e9315210badc4f106ab63c4d2.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/a/a/3/aa3494457a344b93ee982cac609128f1.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/cache/images/8/8/a/88a4071286080cf6cbeef799091d9011.320.240.crop.jpg</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/facebook.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/vk.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/twitter.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/gplus.png</fragment>
    <fragment type="image">https://uahotels.info/components/main/footer/images/rss.png</fragment>
  </page>
</data>

task2.xml
<?xml version='1.0' encoding='UTF-8'?>
<shop>
  <product>
    <description>
                            Роза Перпл Рейн. (Purple Rain) Почвопокровная В КОНТЕЙНЕРЕ                        </description>
    <price>90.00 грн</price>
    <image>/themes/zvetsad/app/pic/logo.png</image>
  </product>
  <product>
    <description>
                            Роза Пэт Остин. (Pat Austin) АНГЛ                        </description>
    <price>65.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/news/1415090772.jpg</image>
  </product>
  <product>
    <description>
                            Роза Пис енд Лав/Мир и Любовь (Брайт эз э Баттон). (Peace and Love) Шраб                        </description>
    <price>50.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/news/1414425983.JPG</image>
  </product>
  <product>
    <description>
                            Роза Сноуфлэйк. (Snowflake) Спрей В КОНТЕЙНЕРЕ                        </description>
    <price>90.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1487756844.jpg</image>
  </product>
  <product>
    <description>
                            Роза Дортмунд. (Dortmund) Плетистая                        </description>
    <price>50.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1576337937.jpg</image>
  </product>
  <product>
    <description>
                            Роза Бургунди Айс. (Burgundy Ice) Флорибунда В КОНТЕЙНЕРЕ                        </description>
    <price>95.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1576180931.jpg</image>
  </product>
  <product>
    <description>
                            Роза Вайт Охара. (White O'Hara) Ч/Г (Однолетний саженец)                        </description>
    <price>55.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1576330140.jpg</image>
  </product>
  <product>
    <description>
                            Роза Пэт Остин. (Pat Austin) АНГЛ. В КОНТЕЙНЕРЕ                        </description>
    <price>95.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1576342985.jpg</image>
  </product>
  <product>
    <description>
                            Роза Арифа. (Arifa)  Спрей (Однолетемй саженец)                        </description>
    <price>50.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1576353557.jpg</image>
  </product>
  <product>
    <description>
                            Роза Майнауфойе. (Mainaufeuer) Почвопокровная В КОНТЕЙНЕРЕ                        </description>
    <price>90.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1578081637.png</image>
  </product>
  <product>
    <description>
                            Роза Фаер Крек. (Fire Crack) Ч/Г В КОНТЕЙНЕРЕ                        </description>
    <price>90.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1576337928.jpg</image>
  </product>
  <product>
    <description>
                            Роза Свит Лагуна/Сладкая Лагуна. (Sweet Laguna) Плетистая                        </description>
    <price>70.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1449481620.jpg</image>
  </product>
  <product>
    <description>
                            Роза Фо Гуд Пич НА ШТАМБЕ                        </description>
    <price>650.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1487789472.jpg</image>
  </product>
  <product>
    <description>
                            Роза Шакенборг. (Schackenborg) Флорибунда В КОНТЕЙНЕРЕ                        </description>
    <price>95.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1575803852.jpg</image>
  </product>
  <product>
    <description>
                            Роза мускусная Ватерлоо. (Rosa rugosa Waterloo) Шраб В КОНТЕЙНЕРЕ                        </description>
    <price>90.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1578589717.png</image>
  </product>
  <product>
    <description>
                            Роза мускусная Ватерлоо. (Rosa rugosa Waterloo) Шраб                        </description>
    <price>55.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1584638468.jpg</image>
  </product>
  <product>
    <description>
                            Роза Ред Каскад. (Red Cascade) Почвопокровная В КОНТЕЙНЕРЕ                        </description>
    <price>90.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1576353980.jpg</image>
  </product>
  <product>
    <description>
                            Роза Вильям Моррис.  (William Morris) АНГЛ В КОНТЕЙНЕРЕ                        </description>
    <price>95.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1588672886.png</image>
  </product>
  <product>
    <description>
                            Роза Грусс Энд Хайдельберг. (Gruss an Heidelberg) Плетистая В КОНТЕЙНЕРЕ                        </description>
    <price>95.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1505242632.png</image>
  </product>
  <product>
    <description>
                            Роза Гай Савой НА ШТАМБЕ                        </description>
    <price>650.00 грн</price>
    <image>https://www.zvetsad.com.ua/upload/catalog/1512546333.jpg</image>
  </product>
</shop>
