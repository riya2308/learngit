package com.bankofmontreal.model;

public abstract class Product {
    private final String productId;

    protected Product(String productId) {
        this.productId = productId;
    }

    public String getProductId() {
        return productId;
    }

    public abstract String getProductType();
}


package com.bankofmontreal.model;

public class Stock extends Product {
    private final String ticker;
    private final String exchange;

    public Stock(String productId, String ticker, String exchange) {
        super(productId);
        this.ticker = ticker;
        this.exchange = exchange;
    }

    @Override
    public String getProductType() {
        return "Stock";
    }
}


package com.bankofmontreal.model;

public class Future extends Product {
    private final String exchange;
    private final String contractCode;
    private final int month;
    private final int year;

    public Future(String productId, String exchange, String contractCode, int month, int year) {
        super(productId);
        this.exchange = exchange;
        this.contractCode = contractCode;
        this.month = month;
        this.year = year;
    }

    @Override
    public String getProductType() {
        return "Future";
    }
}


package com.bankofmontreal.pricing;

import com.bankofmontreal.model.Product;

public interface PricingService {
    double price(Product p);
}


package com.bankofmontreal.pricing;

import com.bankofmontreal.model.Product;

public class FuturePricingService implements PricingService {
    @Override
    public double price(Product p) {
        return 2.0;
    }
}


package com.bankofmontreal.pricing;

import com.bankofmontreal.model.Product;

public class StockPricingService implements PricingService {
    @Override
    public double price(Product p) {
        return 1.0;
    }
}


package com.bankofmontreal.pricing;

import com.bankofmontreal.model.Product;

import java.util.HashMap;
import java.util.Map;

public class PricingServiceFactory {
    private static final Map<String, PricingService> serviceCache = new HashMap<>();

    static {
        serviceCache.put("Stock", new StockPricingService());
        serviceCache.put("Future", new FuturePricingService());
    }

    public static PricingService getInstance(Product product) {
        return serviceCache.get(product.getProductType());
    }
}


package com.bankofmontreal.trade;

import com.bankofmontreal.model.Product;

public interface MontrealTradedProducts {
    void registerNewProduct(Product product) throws ProductAlreadyRegisteredException;
    void trade(Product product, int quantity);
    int totalTradeQuantityForDay();
    double totalValueOfDaysTradedProducts();
}



package com.bankofmontreal.trade;

import com.bankofmontreal.model.Product;
import com.bankofmontreal.pricing.PricingServiceFactory;

import java.util.HashMap;
import java.util.Map;

public class BankOfMontreal implements MontrealTradedProducts {
    private final Map<String, Integer> productQuantity = new HashMap<>();
    private final Map<String, Product> registeredProducts = new HashMap<>();

    @Override
    public void registerNewProduct(Product product) throws ProductAlreadyRegisteredException {
        if (registeredProducts.containsKey(product.getProductId())) {
            throw new ProductAlreadyRegisteredException();
        }
        registeredProducts.put(product.getProductId(), product);
    }

    @Override
    public void trade(Product product, int quantity) {
        if (!registeredProducts.containsKey(product.getProductId())) {
            return;
        }
        productQuantity.put(product.getProductId(), productQuantity.getOrDefault(product.getProductId(), 0) + quantity);
    }

    @Override
    public int totalTradeQuantityForDay() {
        return productQuantity.values().stream().mapToInt(Integer::intValue).sum();
    }

    @Override
    public double totalValueOfDaysTradedProducts() {
        return productQuantity.entrySet().stream()
                .mapToDouble(entry -> PricingServiceFactory.getInstance(registeredProducts.get(entry.getKey()))
                        .price(registeredProducts.get(entry.getKey())) * entry.getValue())
                .sum();
    }
}


package com.bankofmontreal.trade;

public class ProductAlreadyRegisteredException extends Exception {
}


package com.bankofmontreal.runner;

import com.bankofmontreal.model.Future;
import com.bankofmontreal.model.Product;
import com.bankofmontreal.model.Stock;
import com.bankofmontreal.trade.BankOfMontreal;
import com.bankofmontreal.trade.MontrealTradedProducts;
import com.bankofmontreal.trade.ProductAlreadyRegisteredException;

public class Runner {
    public static void main(String[] args) throws ProductAlreadyRegisteredException {
        Product ibm = new Stock("1", "IBM", "NYSE");
        Product ms = new Stock("2", "MS", "BATS");
        Product meta = new Future("3", "NYSE", "META-2050F", 1, 2050);

        MontrealTradedProducts bom = new BankOfMontreal();

        bom.registerNewProduct(ibm);
        bom.registerNewProduct(ms);
        bom.registerNewProduct(meta);

        try {
            bom.registerNewProduct(meta);
        } catch (ProductAlreadyRegisteredException e) {
            System.err.println("Product already registered.");
        }

        bom.trade(ibm, 10);
        bom.trade(ibm, 20);
        System.out.println("IBM: " + bom.totalTradeQuantityForDay() + ", " + bom.totalValueOfDaysTradedProducts());

        bom.trade(ms, 30);
        bom.trade(ms, 40);
        System.out.println("IBM, MS: " + bom.totalTradeQuantityForDay() + ", " + bom.totalValueOfDaysTradedProducts());

        bom.trade(meta, 50);
        bom.trade(meta, 60);
        System.out.println("IBM, MS, META-2050F: " + bom.totalTradeQuantityForDay() + ", " + bom.totalValueOfDaysTradedProducts());
    }
}
