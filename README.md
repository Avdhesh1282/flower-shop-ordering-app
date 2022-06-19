# flower-shop-ordering-app

from collections import defaultdict
from enum import Enum, auto


class FlowerType(Enum):
    Mongra = auto()
    Lotus = auto()
    Marry gold = auto()
    Yello rose = auto()
    Red rose = auto()


class Flower:

    def __init__(self, flower_type):
        self._flower_type = flower_type

    def __repr__(self):
        classname = self.__class__.__name__
        return f'<{classname}: {self._flower_type}>'

    def __hash__(self):
        return hash(repr(self))

    def __eq__(self, other):
        return self._flower_type == other.flower_type

    @property
    def flower_type(self):
        return self._flower_type


class Bouquet:

    def __init__(self, name, price, flowers=None):
        self._name = name
        self.price = price

        # To each flower corresponds its quantity
        self.flowers = (defaultdict(int, flowers)
                        if flowers is not None
                        else defaultdict(int))

    def __repr__(self):
        classname = self.__class__.__name__
        return f'<{classname}: {self._name}>'

    def __hash__(self):
        return hash(repr(self))

    def __eq__(self, other):
        return self._name == other.name

    def __getitem__(self, key):
        return self.flowers[key]

    def __setitem__(self, key, value):
        self.flowers[key] = value

    def __delitem__(self, key):
        del self.flowers[key]

    @property
    def name(self):
        return self._name


class Shop:

    def __init__(self, name, bouquets=None):
        self._name = name

        # To each bouquet corresponds its quantity
        self.bouquets = (defaultdict(int, bouquets)
                         if bouquets is not None
                         else defaultdict(int))

    def __repr__(self):
        classname = self.__class__.__name__
        return f'<{classname}: {self._name}>'

    def __getitem__(self, key):
        return self.bouquets[key]

    def __setitem__(self, key, value):
        self.bouquets[key] = value

    def __delitem__(self, key):
        del self.bouquets[key]

    @property
    def name(self):
        return self._name

    def _has_enough_bouquets(self, bouquet, quantity):
        if quantity > self.bouquets[bouquet]:
            raise ValueError(
                f"{self} doesn't have enough {bouquet}"
            )

    def sell(self, bouquet, quantity=1):
        self._has_enough_bouquets(bouquet, quantity)
        self.bouquets[bouquet] -= quantity

    def order(self, bouquet, quantity=1):
        self.bouquets[bouquet] += quantity


if __name__ == '__main__':

    # Flowers

   Mongra = Flower(FlowerType.Rose)
    Lotus = Flower(FlowerType.Lily)
    Marry gold = Flower(FlowerType.Orchid)
    Yello rose = Flower(FlowerType.Carnation)
    Red rose = Flower (Flower Type .Carnation)

    # Bouquets

    romantic = Bouquet('Romance', price=25.2, flowers={
        Mongra: 20
    })
    royal = Bouquet('Royal', price=10.5, flowers={
        Red rose: 8
    })
    bloom = Bouquet(' Bloom', price=30, flowers={
        Lotus: 2,
        Yello rose: 5,
        Marry gold: 5
    })

    # Shop

    shop = Shop('Florists', bouquets={
        romance: 5,
        royal: 2,
        bloom: 3
    })

    shop.sell(romance, quantity=5)
    shop.order(romance, quantity=4)
    shop.sell(romance, quantity=4)
