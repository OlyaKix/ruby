require 'open-uri'
require 'nokogiri'
require 'csv'
url = 'https://gsm-komplekt.ua/category/zapchasti-dlja-apple-ipad/'
file = "file.csv"
class Parser
  attr_accessor :content
  def initialize
    @content=""
  end
  def write_csv_headers(file_name)
    CSV.open(file_name, "w") do |csv|
      csv << ["Товар", "Ціна"]
    end
  end
  def get_data_write_to_csv(url, file_name)
    write_csv_headers(file_name)
    get_content(url)
    write_to_csv_file("file.csv")
    (2..10).each do |number|
      get_content("#{url}?p=#{number}/")
      write_to_csv_file(file_name)
    end
  end
  def get_content(url)
    html = open(url) { |result| result.read }
    @content = Nokogiri::HTML(html)
  end
  def get_price(item)
    item.css("span.product-price").text.strip.chop
  end
  def get_title(item)
    item.css("a.product-name").text.strip.chop
  end
  def write_to_csv_file(file_name)
    CSV.open(file_name, "a") do |csv|
      @content.css("ul.product_list > li").each do  |item|
        begin
        title = get_title(item)
        price = get_price(item).split(" ", -1)[0] + " грн"
        csv << [title, price]
        rescue
        end
      end
    end
  end
end
parser_object = Parser.new
parser_object.get_data_write_to_csv(url, file)